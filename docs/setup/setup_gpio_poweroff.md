Control both an external LED and a 5V relay module to monitor your Raspberry Pi 5 shutdown status.
Use the GPIO Overlay method to configure the LED and relay as status indicators that flip states the exact second the operating system finishes its safe shutdown routine.

## Step 1: Wire the Components
You can wire both the LED and the relay module to the same ground, but they will pull power from different pins based on their electrical needs.

- **For the Status LED:** Wire it in series with a 220Ω to 330Ω resistor to limit current.
- **For the Relay Module:** Connect its VCC pin to the Pi's 5V rail so the heavy mechanical coil gets plenty of power.

**Hardware Pin Connections**

| Component Pin	| Raspberry Pi 5 Target Pin | Purpose |
| :--- | :--- | :--- |
|**LED Anode (+ long leg)** | **Physical Pin 16** (GPIO 23) | Logic status output |
|**LED Cathode (- short leg)** | Connect to one side of a 220Ω resistor. Connect the other side of the resistor to Physical Pin 14 (GND). | Ground path |
|**Relay VCC** | **Physical Pin 2 or 4** (5V Power) | Power for the relay coil |
|**Relay GND** | **Physical Pin 20** (GND) | Ground reference |
|**Relay IN (Signal)** | **Physical Pin 18** (GPIO 24) | Logic status output|

## Step 2: Configure the Software Overlays
Because you have two distinct physical devices (the LED and the relay), you can utilize two separate built-in system overlays inside Raspberry Pi OS.

1. Open your terminal and edit the firmware configuration file:
    ```bash
    sudo nano /boot/firmware/config.txt
    ```
1. Scroll to the absolute bottom of the file and paste the following two lines:
    ```bash
    dtoverlay=act-led,gpio=23
    dtoverlay=gpio-poweroff,gpiopin=24,active_low
    ```
1. Save and close the file by pressing `Ctrl+X`, then `Y`, then `Enter`.
1. Reboot your system to activate the changes:
    ```bash
    sudo reboot
    ```
## How Your Circuit Behaves Now
**While the Pi 5 is Running**

- **The LED (GPIO 23):** Acts as your active system light. It will flicker dynamically with green activity whenever the Pi reads or writes data.
-  **The Relay (GPIO 24):** The Pi drives GPIO 23 **HIGH**. If using an active-low relay module, a HIGH signal keeps the relay **Open (Off)**.

**The Moment the Pi Shuts Down**

1. When you run `sudo poweroff`, the OS safely parks your system data.
1. The exact millisecond the software finishes halting, the Linux kernel pulls GPIO 24 **LOW (0V)**.
1. This drops the signal to the relay module, immediately **Closing (Turning On) the relay**.
1. At the same moment, the `act-led` controller relinquishes GPIO 23, and the **LED turns completely off**.

You can hook external electronics up to the relay's **Normally Open (NO)** or **Normally Closed (NC)** terminals depending on whether you want an external device to turn *On* or *Off* when the Pi dies.