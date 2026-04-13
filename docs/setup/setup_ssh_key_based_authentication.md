Setup **SSH Key-Based Authentication** to replace the password prompt with a digital "handshake" between a private key on your Local host and a public key on a Remote host.

!!! success "Can the public key be used on more than one Remote host?"
	SSH keys are not "locked" to a specific destination device. Think of your SSH key pair like a physical key and a lock: your Local host holds the **private key**, and you can install the **public key** (the "lock") on as many Raspberry Pis, servers, or cloud instances as you like.

## 1. Generate an SSH Key Pair on Local host

Right-click on PowerShell or Command Prompt and select **Run as Administrator** to run the following command. When prompted to "Enter file in which to save the key," just press **Enter** to use the default location. You can also skip the passphrase by pressing **Enter** twice.

```powershell
# For GitHub Remote host, enter:
ssh-keygen -t ed25519 -C "krsmith436@att.net"

# For Raspberry Pi Remote host, enter:
ssh-keygen -t rsa -b 4096
```

This creates two files in your `.ssh` folder (usually `C:\Users\YourName\.ssh`):
- `id_rsa`: Your **Private Key** (Keep this safe and never share it).
- `id_rsa.pub`: Your **Public Key** (This is what we will give to the Remote host).

## 2. Start the SSH agent

```powershell
# Start the service
Get-Service ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent

# For GitHub, add the key
ssh-add $HOME\.ssh\id_ed25519
```
Command breakdown:
- `Get-Service ssh-agent` - Finds the SSH agent service
- `|` - Pipes (sends) that service to the next command
- `Set-Service -StartupType Automatic` - Changes its startup configuration to Automatic

You only need to run this once, and the setting persists across reboots.

## 3. Copy the Public Key to the Remote host

### a) For GitHub

- Copy the public key

	```powershell
	Get-Content $HOME\.ssh\id_ed25519.pub | Set-Clipboard
	```
	Copy the entire output (starts with `ssh-ed25519`).

- Go to GitHub.com → Settings → SSH and GPG keys
- Click "New SSH key"
- Paste your public key and give it a title, i.e. `My Acer Pc`
- Click "Add SSH key"

### b) For Raspberry Pi

Since Windows doesn't always have the `ssh-copy-id` tool found on Linux, the easiest way to do this via PowerShell is with this command (replace `username` and `<IP_ADDRESS>` with your details):

```powershell
type $env:USERPROFILE\.ssh\id_rsa.pub | ssh username@<IP_ADDRESS> "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

!!! note "Note"
	- You will have to enter your password **one last time** to complete this step.
	- This command takes your public key, logs into the Pi, creates the `.ssh` folder if it doesn't exist, and appends your key to the `authorized_keys` file.

## 4. Test the connection

### a) For GitHub

```powershell
ssh -T git@github.com
```

When asked, "The authenticity of host... can't be established. Are you sure you want to continue?", type `yes` and hit `Enter`.<br>
You should then see, `Hi username! You've successfully authenticated...`

### b) For Raspberry Pi

Now, try to use `scp` or `ssh` again:

```bash
scp C:\path\to\file.txt username@<IP_ADDRESS>:/home/username/
```

The file should transfer immediately without asking for a password.

!!! tip "Quick Troubleshooting"
	- **Permissions:** If it still asks for a password, log into your Pi and ensure the permissions are strict (SSH requires this for security):
		```bash
		chmod 700 ~/.ssh
		chmod 600 ~/.ssh/authorized_keys
		```
	- **Multiple Keys:** If you have multiple SSH keys on your Windows machine, you may need to specify which one to use with the `-i` flag:
		`scp -i C:\Users\YourName\.ssh\id_rsa file.txt username@IP:/path/`

## 5. Use SSH for GitHub repositories

When cloning, use the SSH URL:

```powershell
git clone git@github.com:username/repository.git
```

## 6. Setup the SSH config File

Setting up an SSH config file is the best way to streamline your workflow. It allows you to replace a long command like `scp file.py pi@192.168.1.50:/home/pi/` with something as simple as `scp file.py smithsvalley:`.

### a) Create the config File
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

### b) Add Your Raspberry Pi Details
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



### c) Usage
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

!!! tip "VS Code Integration"
	If you use **Visual Studio Code** for your Python development, you can install the **"Remote - SSH"** extension. It will automatically read this `config` file, allowing you to open a folder directly on your Raspberry Pi and edit code as if it were stored locally on your PC. This completely removes the need to manually `scp` files back and forth.

## 7. Multiple Remote hosts using one private key

Since a key pair has been generated on your Local host, you just need to get the **public** portion onto your new Remote host.

!!! warning "Security Alert"
	Since you are using the same key for multiple devices, remember that if your Windows PC's private key is ever compromised, **all** your Pis are accessible to the attacker.

### Option 1: The "Easy" Way (Command Line)

Open PowerShell or Command Prompt on your Windows PC and run:

```powerShell
ssh-copy-id username@raspberrypi5.local
```

!!! note "Note"
	If Windows doesn't recognize `ssh-copy-id`, you can manually pipe the key over:
	<br> `cat ~/.ssh/id_rsa.pub | ssh username@raspberrypi5.local "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"`

### Option 2: The Manual Way

1.  **On Windows:** Open your public key file (usually located at `C:\Users\YourName\.ssh\id_rsa.pub`) with Notepad and copy the entire string of text.
2.  **On the Pi 5:** Log in (using a password for now) and open the authorized keys file:
    `nano ~/.ssh/authorized_keys`
3.  **Paste** your key on a new line, save (Ctrl+O, Enter), and exit (Ctrl+X).

### Update config File

To make life easier, edit the `config` file on Local host (`~/.ssh/config`) to give them nicknames:

```text
Host pi5
    HostName 192.168.1.158
    User krsmith436
    IdentityFile ~/.ssh/id_rsa

Host zero
    HostName 192.168.1.117
    User krsmith436
    IdentityFile ~/.ssh/id_rsa
```

Now you can just type `ssh pi5` and you're in!