To enable the interface, type `sudo systemctl enable pypilot_hat`.

To make it work without the hat, edit `/etc/systemd/system/pypilot_hat.service` and change '44444' into 'none'

Then it should look like this:
```
pi@openplotter:~/pypilot $ pypilot_hat none
have gpio for raspberry pi
loading config file: /home/pi/.pypilot/hat.conf
failed to load /proc/device-tree/hat/custom_0 : [Errno 2] No such file or directory: '/proc/device-tree/hat/custom_0'
overriding driver default  to command line none
arduino process on  28138
renice: failed to set priority for 28138 (process ID): Permission denied
warning, failed to renice hat arduino process
No hat config, arduino not found
28139 (process ID) old priority 0, new priority 19
(30 seconds pause)
web process 28139
web config {'remote': False, 'host': 'pypilot', 'actions': {'auto': [], 'menu': [], 'up': [], 'down': [], 'select': [], 'left': [], 'right': [], 'engage': ['gpio18'], 'disengage': ['gpio23'], '1': [], '-1': [], '2': [], '-2': [], '10': [], '-10': [], 'compassmode': [], 'gpsmode': [], 'windmode': [], 'tackport': [], 'tackstarboard': []}, 'pi.ir': False, 'arduino.ir': False, 'arduino.nmea.in': False, 'arduino.nmea.out': False, 'arduino.nmea.baud': 4800, 'lcd': {'contrast': 60, 'invert': False, 'backlight': 200, 'flip': False, 'language': 'en', 'bigstep': 10, 'smallstep': 1, 'remote': False, 'hue': 27}}
web process on port 33333
web client connected c9fb01fcb0ac415a91e56d8432098268
apply ('gpio23', 1) 168451.776421598
apply ('gpio23', 2) 168451.776693334
apply ('gpio23', 0) 168451.776797443
```

Note that the web interface at port :33333 has a startup delay of 30 seconds.

If you don't have an infrared sensor, set pi.ir to False in ~/.pypilot/hat.conf


***
[Step 6: The Arduino controller >>>](Step-6-The-Arduino-controller)