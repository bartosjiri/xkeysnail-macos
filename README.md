# Xkeysnail macOS config
A Xkeysnail config that emulates macOS styled keyboard controls/shortcuts in Linux and X11.

## Installation
1. Install Xkeysnail using your package manager or following [Xkeysnail installation](https://github.com/mooz/xkeysnail#installation) instructions.

2. Clone or download the provided config file to a location of your preference.

3. Edit the config file and replace `Your-terminal-app-here` with the window class of your terminal application.

   *To get the window class name, run `xprop WM_CLASS` and click on the window of the terminal application.*  

4. Create a systemd service for xkeysnail to run automatically on the background:

```
cd /etc/systemd/system && sudo nano xkeysnail.service
```

5. Insert the following code to the service config and edit the path to the provided config file:
```
[Unit]
Description=xkeysnail

[Service]
Type=simple
KillMode=process
ExecStart=/usr/bin/sudo /usr/bin/xkeysnail --quiet --watch /path/to/your/config-macos.py
ExecStop=/usr/bin/sudo /usr/bin/killall xkeysnail
Restart=on-failure
RestartSec=3
Environment=DISPLAY=:0

[Install]
WantedBy=graphical.target
```

6. Enable the xkeysnail service:
```
sudo systemctl enable xkeysnail
```

7. Start the xkeysnail service:
```
sudo systemctl start xkeysnail
```


## Troubleshooting 
To check if the xkeysnail service is running properly, run:
```
sudo systemctl status xkeysnail
```

- If you encounter errors like `Xlib.error.DisplayConnectionError: Can't connect to display ":0.0": b'No protocol specified\n'`, make sure you have `xhost` package installed and try:
```
sudo xhost + && sudo systemctl restart xkeysnail
```

## Limitations
- Command key input in terminals is troublesome, since macOS uses <kbd>ctrl</kbd> for all terminal operations. While the provided scripts provide a workaround for this, you won't be able to use these in built-in terminals (i.e. VS Code).
