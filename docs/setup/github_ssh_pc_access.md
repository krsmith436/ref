Setup **SSH Key-Based Authentication** to replace the password prompt with a digital "handshake" between a private key on your Windows PC and a public key on GitHub.

---

## 1. Generate an SSH key
Right-click on PowerShell or Command Prompt and select **Run as Administrator**:
```powershell
ssh-keygen -t ed25519 -C "krsmith436@att.net"
```
Press Enter to accept the default file location (`C:\Users\YourUsername\.ssh\id_ed25519`), then optionally set a passphrase.
## 2. Start the SSH agent
```powershell
# Start the service
Get-Service ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent

# Add your key
ssh-add $HOME\.ssh\id_ed25519
```
Command breakdown:
- `Get-Service ssh-agent` - Finds the SSH agent service
- `|` - Pipes (sends) that service to the next command
- `Set-Service -StartupType Automatic` - Changes its startup configuration to Automatic

You only need to run this once, and the setting persists across reboots.
## 3. Copy your public key
```powershell
Get-Content $HOME\.ssh\id_ed25519.pub | Set-Clipboard
```
Copy the entire output (starts with `ssh-ed25519`).
## 4. Add the key to GitHub
- Go to GitHub.com → Settings → SSH and GPG keys
- Click "New SSH key"
- Paste your public key and give it a title
- Click "Add SSH key"
## 5. Test the connection
```powershell
ssh -T git@github.com
```
When asked, "The authenticity of host... can't be established. Are you sure you want to continue?", type `yes` and hit `Enter`.<br>
You should then see, `Hi username! You've successfully authenticated...`
## 6. Use SSH for repositories
When cloning, use the SSH URL:
```powershell
git clone git@github.com:username/repository.git
```
For existing repos using HTTPS, switch to SSH:
```powershell
git remote set-url origin git@github.com:username/repository.git
```
That's it! You're now set up to use SSH with GitHub on Windows 11.