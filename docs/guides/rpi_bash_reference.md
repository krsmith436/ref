# Raspberry Pi Bash Reference For krsmith436

## USE ALIAS TO MAKE IT AUTOMATIC
If your Pi is "headless" (no monitor) and just sitting in a corner, you might get tired of SSHing into it just to type `git pull`.

You can create a simple **alias** so you only have to type one word to update and run your project. Open your bash configuration:
`nano ~/.bashrc`

Add this line at the bottom:

```Bash
alias update-pi='cd ~/btferret && git pull && sudo python3 main.py'
```

(Save and exit, then run `source ~/.bashrc`)

Now, whenever you type `update-pi`, your Pi will automatically grab the latest code and start the script.
