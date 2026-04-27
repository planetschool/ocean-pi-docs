---
sidebar_position: 7
---

# 7. Running Your Buoy On Boot

Run `sudo nano /etc/systemd/system/oceanpi.service` in terminal. This will create a new empty file that you should paste the following into:

```bash
[Unit]
Description=Run code at boot
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=planetschool
WorkingDirectory=/home/planetschool/ocean-pi
ExecStartPre=/bin/sleep 15
ExecStart=/home/planetschool/venv-ocean-pi/bin/python3 /home/planetschool/ocean-pi/Ocean-Pi-Buoy-Project.py>
Restart=on-failure
RestartSec=10
Environment=PYTHONUNBUFFERED=1
Environment=THINGSBOARD_TOKEN=your_token_here

[Install]
WantedBy=multi-user.target
```

There are several items in the code above that you need to change for your personal Raspberry Pi and your project:
* Your Raspberry Pi username needs to go in `User`
* I store all my Ocean Pi files in a folder called "ocean-pi". Whatever folder (directory) you have your project files in needs to go in `WorkingDirectory`
* The name of your virtual environment needs to go into `ExecStart` where I have `venv-ocean-pi`, which is the name of my virtual environment
* The filepath of your code needs to replace `/home/planetschool/ocean-pi/Ocean-Pi-Buoy-Project.py`
* `THINGSBOARD_TOKEN` needs to have your secret token code

Once all of that is updated, follow the same procedure from when you last used Nano to edit the .bashrc file. Hit Ctrl+X, type "Y" to save, then hit "Enter/Return" to keep the filename "oceanpi.service".

We have just created a service that will run when the Raspberry Pi boots up. Now we need to activate it. Run the following commands in terminal:

```bash
sudo systemctl daemon-reload
sudo systemctl enable oceanpi.service
sudo systemctl start oceanpi-service
journalctl -u oceanpi.service -f --no-pager
```