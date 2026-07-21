Using a Wii Nunchuck as a mouse on a Pi comes down to three things: the Nunchuck is an I2C device, so you wire it to the Pi's I2C pins and read its joystick/button bytes; then you translate those into pointer movement and clicks using a virtual input device (`/dev/uinput`). Here's the full path, with a Python script since that's your language.

## 1. Wire it up

The Nunchuck runs on 3.3V, which matches the Pi's I2C levels, so no level shifting is needed.<br>
A **XYAB 3rd Party Nunchuck** from **Retro-Taku Videogames Walled Lake** is being used.

Connect to the Pi's 40\-pin header and Nunchuck:

| Signal | Pi 40-pin | Amp 4-pin | Nunchuck Wire Color |
| :--- | :---: | :---: | :--- |
| **3.3V** | 1 | 2 | Red & Black | 
| **GND** | 6 | 1 | Green |
| **Data (SDA)** | 3 | 3 | Yellow |
| **Clock (SCL)** | 5 | 4 | White |

The Pi already has pull-ups on the I2C lines, so you generally don't need to add any.

## 2. Enable I2C and verify the device

Run `sudo raspi-config` → Interface Options → I2C → Enable, then reboot. Install the tools and confirm the Nunchuck appears at address `0x52`:

```bash
sudo apt install i2c-tools
i2cdetect -y 1
```

You should see `52` in the grid. If nothing shows up, recheck wiring (SDA/SCL swapped is the usual culprit).

## 3. Install Python dependencies

```bash
sudo apt install python3-smbus2 python3-evdev
```

On Bookworm, if either package isn't available via apt, use a virtual environment or `pip install --break-system-packages smbus2 evdev` instead.

## 4. Allow writing to /dev/uinput

The script creates a virtual mouse through `/dev/uinput`, which needs the kernel module loaded and write access:

```bash
sudo modprobe uinput
```

Simplest path is to run the script with `sudo`. If you'd rather run it as your normal user, create `/etc/udev/rules.d/99-uinput.rules` containing `KERNEL=="uinput", MODE="0660", GROUP="input"`, add yourself with `sudo usermod -aG input $USER`, and add `uinput` to `/etc/modules` so it loads on boot.

## 5. The script

A couple of notes on how it works: the init sequence (`0xF0 0x55`, `0xFB 0x00`) disables the Nunchuck's data encryption, so the six data bytes can be read straight without decoding. Byte 0 and 1 are joystick X/Y (center ≈ 128), and byte 5 holds the C and Z button bits. I mapped Z (the big trigger) to left-click and C to right-click.Run it with `sudo python3 nunchuck_mouse.py` and the joystick will drive the pointer, Z is left-click, C is right-click. Tweak `SPEED` and `DEADZONE` at the top to get the feel right.

A few things worth knowing: the joystick center isn't always exactly 128, so if the pointer drifts when you're not touching it, print `joy_x, joy_y` at rest and adjust `CENTER` (or bump `DEADZONE`). To launch it automatically at boot, wrap it in a small systemd service. And if you later want the accelerometer too (for tilt-based movement), bytes 2–4 hold the X/Y/Z acceleration with the low bits packed into byte 5.

## 6. Run script at boot

A systemd service is the clean way to do this — it starts the script at boot, restarts it if it ever crashes, and gives you logs.

1. Copy the Python script to my `/bin` folder:

    ```bash
    sudo cp nunchuck_mouse.py ~/bin/
    ```

1. Create the `nunchuck-mouse.service` file with the following content:
    ```bash
    [Unit]
    Description=Wii Nunchuck as mouse
    After=multi-user.target

    [Service]
    Type=simple
    # Make sure the virtual-input kernel module is loaded before we start.
    ExecStartPre=/sbin/modprobe uinput
    ExecStart=/usr/bin/python3 /home/krsmith436/bin/nunchuck_mouse.py
    Restart=on-failure
    RestartSec=3

    [Install]
    WantedBy=multi-user.target
    ```
1. Copy the `.service` file to my `/bin` folder:

    ```bash
    sudo cp nunchuck-mouse.service ~/bin/
    ```

1. Install and enable the service:

    ```bash
    sudo systemctl daemon-reload
    sudo systemctl enable nunchuck-mouse.service
    sudo systemctl start nunchuck-mouse.service
    ```
    The `enable` line is what makes it launch on every boot; the `start` line runs it right now so you can test without rebooting. 

1. Check that it's running with:

    ```bash
    sudo systemctl status nunchuck-mouse.service
    ```

1. To stop it launching at boot later, 
    ```
    sudo systemctl disable --now nunchuck-mouse.service
    ```
    
!!! note "If something's wrong"
    `journalctl -u nunchuck-mouse -f` shows the script's output and any errors live.
