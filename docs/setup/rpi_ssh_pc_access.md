Setup **SSH Key-Based Authentication** to replace the password prompt with a digital "handshake" between a private key on your Windows PC and a public key on your Raspberry Pi.

---

### 1. Generate an SSH Key Pair on Windows
Open **PowerShell** and run the following command. When prompted to "Enter file in which to save the key," just press **Enter** to use the default location. You can also skip the passphrase by pressing **Enter** twice.

```powershell
ssh-keygen -t rsa -b 4096
```

This creates two files in your `.ssh` folder (usually `C:\Users\YourName\.ssh`):
* `id_rsa`: Your **Private Key** (Keep this safe and never share it).
* `id_rsa.pub`: Your **Public Key** (This is what we will give to the Pi).

### 2. Copy the Public Key to the Raspberry Pi
Since Windows doesn't always have the `ssh-copy-id` tool found on Linux, the easiest way to do this via PowerShell is with this command (replace `username` and `<IP_ADDRESS>` with your details):

```powershell
type $env:USERPROFILE\.ssh\id_rsa.pub | ssh username@<IP_ADDRESS> "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

* **Note:** You will have to enter your password **one last time** to complete this step.
* This command takes your public key, logs into the Pi, creates the `.ssh` folder if it doesn't exist, and appends your key to the `authorized_keys` file.

### 3. Test the Connection
Now, try to use `scp` or `ssh` again:

```bash
scp C:\path\to\file.txt username@<IP_ADDRESS>:/home/username/
```

The file should transfer immediately without asking for a password.

---

### Quick Troubleshooting
* **Permissions:** If it still asks for a password, log into your Pi and ensure the permissions are strict (SSH requires this for security):
    ```bash
    chmod 700 ~/.ssh
    chmod 600 ~/.ssh/authorized_keys
    ```
* **Multiple Keys:** If you have multiple SSH keys on your Windows machine, you may need to specify which one to use with the `-i` flag:
    `scp -i C:\Users\YourName\.ssh\id_rsa file.txt username@IP:/path/`