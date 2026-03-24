| 🔗 [Bash Reference Manual](https://www.gnu.org/software/bash/manual/bash.pdf) | 🔗 [Specification for Linux Filesystem](https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.pdf) |


### General Commands

| Command | Meaning |
| :----: | ---- |
| `clear` | Clear terminal screen |
| ⬆️ | Recall previous command |
| ⬇️ | Recall next command |
| `~` | /home/krsmith436 |
| `$` | Normal user |
| `#` | root user |
| `exit` | End session |
| `Ctrl+D` | End session |
| `nano` | Simple text editor |

### My Scripts

| Name | Meaning |
| ---- | ---- |
| `myupdate` | = `apt update && apt upgrade` |
| `mydiagnostics` | Displays system information |

### Services

| Command | Meaning |
| ---- | ---- |
| `sudo apt install python -xyz` | Install `xyz` package |
| `sudo apt autoremove` | Remove packages no longer required |
| `chmod +x script.sh` | Make `script.sh` executable |

### System Services

| Command | Meaning |
| ---- | ---- |
| `sudo systemctl start mosquitto` | Start the `mosquitto`, for example, service |
| `sudo systemctl stop mosquitto` | Stop the `mosquitto` service, to bebug |
| `sudo systemctl disable mosquitto` | To not start the `mosquitto` service automatically at boot |
| `sudo systemctl enable mosquitto` | To start the `mosquitto` service automatically at boot |
| `sudo systemctl status mosquitto` | Displays status of the `mosquitto` service |

!!! tip "Tip for mosquitto service"
    Use the `no-pager` option. If you forget, enter `q` to quit if it doesn't automatically.
	```bash
	sudo systemctl status mosquitto --no-pager
	```

### Use Alias To Make It Automatic
If your Pi is "headless" (no monitor) and just sitting in a corner, you might get tired of SSHing into it just to type `git pull`.

You can create a simple **alias** so you only have to type one word to update and run your project. Open your bash configuration:
`nano ~/.bashrc`

Add this line at the bottom:

```Bash
alias update-pi='cd ~/btferret && git pull && sudo python3 main.py'
```

(Save and exit, then run `source ~/.bashrc`)

Now, whenever you type `update-pi`, your Pi will automatically grab the latest code and start the script.

