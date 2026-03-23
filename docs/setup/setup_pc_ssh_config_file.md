Setting up an SSH config file is the best way to streamline your workflow. It allows you to replace a long command like `scp file.py pi@192.168.1.50:/home/pi/` with something as simple as `scp file.py smithsvalley:`.

---

### 1. Create the Config File
On your Windows 11 machine, the SSH configuration file lives in your user profile's `.ssh` folder.

1.  Open **PowerShell**.
2.  Run this command to create the file (if it doesn't already exist):
    ```powershell
    New-Item -Path "$env:USERPROFILE\.ssh\config" -ItemType File -Force
    ```
3.  Open that file in **Notepad** (or any text editor):
    ```powershell
    notepad "$env:USERPROFILE\.ssh\config"
    ```

### 2. Add Your Raspberry Pi Details
Paste the following block into the Notepad window. Replace the values with your specific details:

```text
Host smithsvalley
    HostName <IP_ADDRESS_OR_HOSTNAME>
    User pi
    IdentityFile ~/.ssh/id_rsa
```

* **Host:** This is the "nickname" you want to use. You can call it `pi`, `zero`, or `smithsvalley`.
* **HostName:** The actual IP address of your Raspberry Pi Zero 2 W (e.g., `192.168.1.50`) or its network name (e.g., `raspberrypi.local`).
* **User:** The username you use to log into the Pi.
* **IdentityFile:** The path to the private key we generated earlier. The `~` symbol correctly points to your user folder.



### 3. Usage
Once you save the file, you no longer need to type the full connection string.

**To SSH into the Pi:**
```bash
ssh smithsvalley
```

**To SCP a file to the Pi:**
```bash
scp script.py smithsvalley:~/projects/
```

**To SCP a file from the Pi to your current Windows folder:**
```bash
scp smithsvalley:~/data.log .
```

---

### Pro Tip: VS Code Integration
If you use **Visual Studio Code** for your Python development, you can install the **"Remote - SSH"** extension. It will automatically read this `config` file, allowing you to open a folder directly on your Raspberry Pi and edit code as if it were stored locally on your PC. This completely removes the need to manually `scp` files back and forth.