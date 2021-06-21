* [Services](#services)
* [Console scripts](#console-scripts)
* [Running pypilot at the prompt](#running-pypilot-at-the-prompt)
* [Processes](#processes)
* [Network](#network)
* [Determining the pypilot version](#determining-the-pypilot-version)
* [Updating the pypilot software](#updating-the-pypilot-software)


There is always great pleasure looking under the hood, so let’s have a good old look. It'll show what you should see, so you can compare it with your own findings if you need to diagnose a problem. This page assumes some linux background knowledge, but if you lack that, try to follow the examples and you will be into it before you know it. To get a prompt on openplotter, click Raspberry→Accessories→Terminal. Remotely, from a windows pc, download putty.exe.

> If you put some diagnostic evidence in your forum questions, you are more likely to get a quick answer. Even better: when you go under the hood first, you might be able to answer your own question even before asking it! 

## Services
Software that runs in the background is typically organized as ‘services’. On openplotter, the pypilot ‘systemd’ service definitions are in /etc/systemd/system/:
```
pi@openplotter:/etc/systemd/system $ ls -1 pypilot*
pypilot_boatimu.service
pypilot.service
pypilot_web.service
```
pypilot.service is the main service. pypilot.boatimu is the compass service, which can run stand-alone. Pypilot_web is the browser interface; it needs the pypilot service to be running.

To check if a service is running, use `systemctl`. In this example, the pypilot service is running. Don't ask questions unless you know for sure that your pypilot service is running:
```
pi@openplotter:~ $ systemctl status pypilot
pypilot.service - pypilot
   Loaded: loaded (/etc/systemd/system/pypilot.service; disabled; vendor preset: enabled)
   Active: active (running) since Thu 2021-03-11 16:50:17 CET; 3s ago
```
## Console scripts
Services run python **console scripts**. These are shell scripts that refer to python scripts inside the python 'egg'<sup>1</sup>. The pypilot console scripts can be found in /usr/local/bin/:
```
pi@openplotter:/usr/local/bin $ ls -1 pypilot*
pypilot
pypilot_boatimu
pypilot_calibration
pypilot_client
pypilot_client_wx
pypilot_control
pypilot_hat
pypilot_scope
pypilot_servo
pypilot_web
```
Normally these scripts are run by the services, but you can run them separately for diagnostics purposes. Some of these scripts require the main pypilot service to be running, some are the main service. Some of them have a GUI, some of them have a CLI. I’m not explaining them all, there is some documentation in the github readme. 

> What is interesting, is that these pypilot scripts default to the pypilot that is running on the 'localhost'. Localhost is IT-speak for 'this computer'. Many of the pypilot scripts can be invoked with _the IP-address of another pypilot that runs remotely_, for example, a tinypilot. So if you have a tinypilot running, you can run an openplotter scope remotely on the tinypilot's data. I might come back later to this, but for now, remember this well: the openplotter user interface components are not limited to a pypilot that runs on openplotter. See [Step 12: Using openplotter tools remotely](Step-12-Using-openplotter-tools-remotely)

## Running pypilot at the prompt
Normally services produce logging in /var/log, but on openplotter I cannot find them. If something is wrong, you might want to to disable pypilot in the openplotter user interface (or stop the service), and then run it at the prompt for user pi. You will see valuable information:
```
pi@openplotter:~ $ pypilot
imu process 4260
nmea process 4265
listening on port 20220 for nmea connections
loading servo calibration /home/pi/.pypilot/servocalibration
WARNING: using default servo calibration!!
Loaded Pilots: ['learning', 'basic', 'absolute', 'simple']
warning: failed to open special file /dev/watchdog0 for writing
         cannot stroke the watchdog
gps process 4267
made imu process realtime
[…] etc.
```
Remember: if you have a problem, and no-one else can help you, run pypilot (or any other console script) at the prompt. Carefully read the output: pypilot is talking to you so you'd better listen. If you want to post the output on the forum, it's best to save it in a text file, then attach the text file to a forum post. Unless it's a short snipplet. In that case, put it between code tags, so a reader knows you are serious, and you are helping them to help you.

## Processes
The quickest test of all, is to look for **processes** that are started by the pypilot scripts. If it does not look like this, you have something to think about for yourself:
```
pi@openplotter:~ $ ps -ef | grep pypilot
pi  4157   805  0 14:24 00:00:02 /usr/bin/python3 /usr/bin/openplotter-pypilot
pi  4527     1  3 14:27 00:00:36 /usr/bin/python /usr/local/bin/pypilot
pi  4529     1  0 14:27 00:00:00 /usr/bin/python3 /usr/bin/openplotter-pypilot-read
pi  4535  4527  0 14:27 00:00:04 /usr/bin/python /usr/local/bin/pypilot
pi  4536  4527  1 14:27 00:00:18 /usr/bin/python /usr/local/bin/pypilot
pi  4542  4527  0 14:27 00:00:00 /usr/bin/python /usr/local/bin/pypilot
pi  4543  4527  0 14:27 00:00:00 /usr/bin/python /usr/local/bin/pypilot
pi  4546  4527  0 14:27 00:00:00 /usr/bin/python /usr/local/bin/pypilot
pi  4548  4527  0 14:27 00:00:06 /usr/bin/python /usr/local/bin/pypilot
pi  4672     1  0 14:34 00:00:01 /usr/bin/python /usr/local/bin/pypilot_web 
pi  4892  1555  0 14:47 00:00:00 grep --color=auto pypilot
```
## Network
Another interesting peek is to look at the **network connections** that pypilot maintains:
```
pi@openplotter:~ $ netstat -antlp|grep python
tcp     0  0 0.0.0.0:23322     0.0.0.0:*           LISTEN      4548/python
tcp     0  0 0.0.0.0:20220     0.0.0.0:*           LISTEN      4542/python
tcp     0  0 0.0.0.0:8080      0.0.0.0:*           LISTEN      4672/python
tcp     0  0 10.10.10.1:23322  10.10.10.1:47460    ESTABLISHED 4548/python
tcp     0  0 127.0.0.1:57390   127.0.0.1:2947      ESTABLISHED 4546/python
```
You can see that pypilot listens out on 23322, 20220 and 8080. The first one is the internal communication, that is used amongst others by the user interfaces to talk to pypilot. The second one is NMEA in/out traffic, we discussed that one already. Do a telnet to this port to see what is happening. 8080 is the browser interface.

You can also see that pypilot reaches out to 2947, which is the gps daemon. You can google that for yourself, but long story short: when you stick a gps in a linux machine, the gpsd connects it to that port.

## Determining the pypilot version
Note that the versions of pypilot mentioned here are different from the versions that are in the openplotter settings screen (from where you installed pypilot). The openplotter tooling has its own versioning scheme, that is unrelated to the pypilot versioning scheme. There is no way to determine the version of pypilot in the user interfaces. The way I check it, is by looking in the pypilot script:
```
pi@openplotter:~ $ which pypilot
/usr/local/bin/pypilot
pi@openplotter:~ $ cat /usr/local/bin/pypilot
#!/usr/bin/python
# EASY-INSTALL-ENTRY-SCRIPT: 'pypilot==0.24','console_scripts','pypilot'
```

The release management of pypilot is not very strict. The version numbering is a rough indication of the generation of the software. Within one version number, there may be many intermediate commits, some of which might break portions of the code. This is no verdict, just an observation and it is good to keep this in mind when you plan to update the pypilot software.

## Updating the pypilot software
If you want to upgrade to the latest development commit of pypilot, issue the following commands. These commands might change over time, so it is always good to check the github readme. At the time of writing (09-APR-2021) the current version is v0.24. Before performing the update, it is always a good idea to bring down pypilot. After the upgrade, run pypilot at the prompt. If something's wrong, you will see right away.

```
cd
sudo systemctl stop pypilot pypilot_web pypilot_hat pypilot_boatimu
sudo rm -Rf pypilot/ pypilot_data/
git clone https://github.com/pypilot/pypilot
cd pypilot
sudo python3 setup.py install
```

***
<sup>1</sup> In case you are really interested in the python distrbibution mechanism, read up [here](https://setuptools.readthedocs.io/en/latest/pkg_resources.html); if you are just nosey just eyeball the console scripts and connect the dots in  `/usr/local/lib/python3.7/dist-packages/pypilot-0.24-py3.7-linux-armv7l.egg/EGG-INFO/entry_points.txt` _mutatis mutandis_.
***

[Step 9: Wiring up the Nano >>>](Step-9-Wiring-up-the-Nano)