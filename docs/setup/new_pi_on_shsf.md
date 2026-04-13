### Install guizero
1.  **Update your package list:**
    ```bash
    sudo apt update
    ```
2.  **Install guizero:**
    ```bash
    sudo apt install python3-guizero
    ```
	
### Install btferret
1. 	**Go to [btferret GitHub](btferret: https://github.com/petzval/btferret) for instructions on how to install.**

2. **Add user to the group**
	Run this to give you permission to manage network interfaces:
	```bash
	sudo usermod -aG bluetooth $USER
	sudo usermod -aG netdev $USER
	```

3. **Update the Terminal Environment Variable**
	a) Open your terminal and open your bash configuration:
	   ```bash
	   nano ~/.bashrc
	   ```
	b) Scroll all the way to the bottom and add this line:
	   ```bash
	   export PYTHONPATH="$PYTHONPATH:/home/krsmith436/python/btferret"
	   ```
	c) Save and exit (`CTRL + O`, `Enter`, `CTRL + X`).
	<br> d) Reload your terminal settings:
	   ```bash
	   source ~/.bashrc
	   ```

4. 	**At the very top of your Python file (before you try to import your library), add this:**
	```python
	import sys

	# Replace with the actual path to your folder
	sys.path.append("/home/krsmith436/python/btferret")

	# Now you can import your module
	import btfpy
	```

5. 	**Navigate to btferret folder and change ownership of all files to current user:**
	```bash
	cd /home/krsmith436/python/btferret
	sudo chown -R krsmith436: .
	```

6. 	**Stop needing sudo for bluetooth**
	- On a raspberry pi zero 2 w,
	```bash
	sudo setcap 'cap_net_raw,cap_net_admin+eip' $(readLink -f $(which python3))
	```
	- On a raspberry pi 5,
	```bash
	sudo setcap 'cap_net_raw,cap_net_admin+eip' /usr/bin/python3.11
	```
	*(Note: Check your version with `python3 --version` to ensure it's 3.11).*

### Install MQTT
| 🔗 [Mosquitto Home Page](https://mosquitto.org/) | 🔗 [Eclipse Mosquitto](https://github.com/eclipse-mosquitto/mosquitto) |

1.  **Update your package list:**
    ```bash
	sudo apt update && sudo apt upgrade -y
    ```
2.  **Install the client/server pre-packaged version:**
    ```bash
    sudo apt install python3-paho-mqtt
    ```
3.  **Install the Broker:**
	```bash
	sudo apt install -y mosquitto mosquitto-clients
	```
	- `mosquitto` is the broker itself.
	- `mosquitto-clients` gives you command-line tools to test your setup.

4. **Configure for External Access:**
	By default, Mosquitto 2.0+ only allows connections from the "localhost" (the Pi itself) for security. To let other devices on your network talk to it, we need to create a configuration file.

	a)  Create a new config file:
		```bash
		sudo nano /etc/mosquitto/conf.d/default.conf
		```
	b)  Paste the following into the file:
		```text
		listener 1883
		allow_anonymous true
		```
	!!! note "Note"
        `allow_anonymous true` is fine for a private home network, but if you're planning to expose this to the internet, you **must** set up a username and password later.
		
	c)  Press `Ctrl + O`, `Enter`, then `Ctrl + X` to save and exit.

5. **Enable and Restart the Service:**
	Now, let’s make sure Mosquitto starts automatically whenever your Pi boots up.

	```bash	
	sudo systemctl enable mosquitto
	sudo systemctl start mosquitto
	```

	To check if it’s running correctly:
	```bash
	sudo systemctl status mosquitto --no-pager
	```
	You should see a glorious green **"active (running)"** status.

6. **Testing the Broker**
	Let's verify that messages are actually moving. You’ll need two terminal windows for this.

	- Terminal 1: The "Subscriber"
	This window will sit and wait for a message on a specific "topic" (we'll call it `test/topic`).
	```bash
	mosquitto_sub -h localhost -t "test/topic"
	```

	- Terminal 2: The "Publisher"
	In this window, send a message to that same topic:
	```bash
	mosquitto_pub -h localhost -t "test/topic" -m "Hello from Pi 5!"
	```

	**The Result:** You should see "Hello from Pi 5!" pop up instantly in the first terminal.

### Pro-Tips for the Pi 5
- **Security:** Once you’re comfortable, look into `mosquitto_passwd` to create a password file. "Anonymous access" is the "leaving your front door unlocked" of the IoT world.
- **Logs:** If something goes wrong, you can check the logs with `tail -f /var/log/mosquitto/mosquitto.log`.