Tinypilot runs under TinyCore Linux. This linux deviates from regular linux distributions. If you want to know the background, you are advised to read a few chapters into the very well written http://www.tinycorelinux.net/corebook.pdf.

- For remote ssh access, log in with username 'tc' and password 'pypilot'.

Take a moment to let sink in what a main difference is between debian derivates and tinycore linux:
- In debian, packages are .deb files and they are installed once from the .deb file onto the read-write root file system. 
- In tinycore linux, packages are .tcz files, and they are loaded at boot-time onto a read-only filesystem, and symbolic links are created to the executables. The script that loads the packages at boot-time, is the script `/opt/bootlocal.sh`. The command that loads .tcz packages is `tce-load`.

>Note that /opt/bootlocal.sh runs in the background, but still outputs its data to the console. So when you look at the boot sequence on the console (if you attach a display to your pi), the shell prompt will be overwritten by the bootlocal.sh output. Pypilot will already be running, while bootlocal.sh continues loading some secondary 'development tools', which might show some error messages. This might cause you to think falsely that something went wrong in the pypilot startup, but if you check the browser interface, you are likely to find that pypilot is already up and running.

Take another moment to understand how the pypilot package (pypilot.tcz!) is loaded in tinycore. Open /opt/bootlocal.sh and eyeball the code. If you understand this, it will make your life so much easier:
```
# This is the pypilot package:
tc@box:~$ find / -name pypilot.tcz 2>/dev/null
/mnt/mmcblk0p2/tce/optional/pypilot.tcz

# This line is where it is being 'loaded' at boot-time:
tc@box:~$ grep pypilot /opt/bootlocal.sh | grep tce-load
chpst -utc tce-load -i python-serial python-RTIMULib python-ujson pypilot > /dev/null

# After loading, the package is availabe at this read-only filesystem:
tc@box:~$ df | grep pypilot
/dev/loop19    384.0K    384.0K    0 100% /tmp/tcloop/pypilot

# And this is how the executables are being accessed: through symbolic links into the read-only filesystem:
tc@box:~$ which pypilot
/usr/local/bin/pypilot
tc@box:~$ ls -al /usr/local/bin/pypilot
lrwxrwxrwx   1 root  root   41 Jun 11 20:00 /usr/local/bin/pypilot -> /tmp/tcloop/pypilot/usr/local/bin/pypilot

services are defined under /etc/sv/
startup with sudo sv {start|stop} servicename
log files are under /var/log/servicename/current

configuration under .pypilot, which links to /mnt/mmcblk0p2/.pypilot
```
## Upgrade pypilot on tinypilot
The easiest way to upgrade tinypilot is to **download the latest tinypilot image** from https://pypilot.org and burn it on the SD card. Take a new SD card so you can roll back to the previous is there is a problem. It should be noted, however, that the latest tinypilot image does not guarantee the latest pypilot sofftware patches. 

But if you understood the previous paragraph, you already know that upgrading pypilot on tinypilot is essentially the same as replacing pypilot.tcz with a new version. Now read this carefully:
>To make a new pypilot.tcz file, clone pypilot from github and run pypilot.build.

**Connecting tinypilot to the internet** - To clone, tinypilot needs to be connected to the internet. If it is connected as a client to the openplotter wi-fi access point, and openplotter is connected to the internet with a cable (or, on the boat, with a USB-tether to your smart phone), you can clone directly from the tinypilot. NOTE: this works if you specified DHCP in the tinypilot wi-fi settings. If you specified a fixed ip address (Client Mode Address), you will get the message `fatal: Unable to look up github.com (port 9418) (Temporary failure in name resolution)`. Understandable, because you have not specified a fixed dns and gateway yet. Make sure there's `nameserver 8.8.8.8` in `/etc/resolve.conf`, and specify a default gateway e.g. `sudo route add default gw 10.10.10.1`

If you done all this, run this code to update pypilot. This is best done line by line, and capture the output of each command as forensic evidence in a text file, that you save with date in the filename.
```
cd
mkdir pypilot-update
cd pypilot-update/
git clone git://github.com/pypilot/pypilot
git clone --depth 1 git://github.com/pypilot/pypilot_data
cp -rv pypilot_data/* pypilot
cd pypilot
. pypilot.build

```
After this, reboot your tinypilot. `sudo reboot` will do, unplugging and replugging the power as well.

**Upgrading tinypilot remotely** - If connecting tinypilot to the internet is a problem, you can also update it remotely from a raspberry that *is* connected to the internet; replace 10.10.10.3 with the ip address of tinypilot:

```
git clone https://github.com/pypilot/pypilot
git clone --depth 1 https://github.com/pypilot/pypilot_data
cp -rv pypilot_data/* pypilot

scp -r pypilot/ tc@10.10.10.3:~/pypilot_update/
ssh tc@10.10.10.3 "cd pypilot_update; . pypilot.build; sudo reboot"
```

**Roll back upgrade** - In both cases, you can revert to the old version of pypilot by running pypilot.build in the old source directory:
```
ssh tc@10.10.10.3 "cd pypilot; . pypilot.build; sudo reboot"
```

## Run pypilot at the prompt
To run pypilot at the prompt, thereby getting instantaneous logging output, stop the pypilot service, and then run the pypilot script. Remember this is the output that gets you targeted help when you post a question on the pypilot forum:
```
tc@box:/mnt/mmcblk0p2/.pypilot$ sudo sv stop pypilot
ok: down: pypilot: 0s
tc@box:/mnt/mmcblk0p2/.pypilot$ pypilot
ERROR loading learning.py No module named tensorflow ,  No module named learning
ERROR loading learning.py No module named tensorflow ,  No module named learning
warning, failed to make calibration process idle, trying renice
Settings file not found. Using defaults and creating settings file
Using settings file RTIMULib.ini
Detected MPU9250/MPU9255 at standard address
Using fusion algorithm Kalman STATE4
IMU Name: MPU-925x
min/max compass calibration not in use
Using ellipsoid compass calibration
Using accel calibration
loading servo calibration /home/tc/.pypilot/servocalibration
WARNING: using default servo calibration!!
connected to gpsd
Loaded Pilots: ['wind', 'simple', 'basic', 'absolute']
warning: failed to open special file /dev/watchdog0 for writing
         cannot stroke the watchdog
MPU-925x init complete
servo is running too _slowly_ 0.0678148269653
value 65535
arduino servo found on [u'/dev/serial/by-id/usb-1a86_USB_Serial-if00-port0', 38400]
serialprobe success: /home/tc/.pypilot/servodevice [u'/dev/serial/by-id/usb-1a86_USB_Serial-if00-port0', 38400]
```


***
[Step 12: Using openplotter tools remotely](Step-12-Using-openplotter-tools-remotely)