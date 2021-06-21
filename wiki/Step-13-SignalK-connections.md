First and for all: SignalK is a big subject. It's a beautiful piece of software that has recently been integrated into openplotter, opencpn and pypilot, and it allows for exchanging nautical data between devices and software components. If you want you can call it a multiplexer, but it is way more than that. It does replace kplex, though. If you want to make a connection with pypilot, you better read up on SignalK first.

There are three ways for getting pypilot sensor data into signalk: **nmea**, **openplotter background script** and **signalk zeroconf**.

## NMEA
The old way is to have signalk **pull** it out of pypilot's nmea port 20220/TCP. The openplotter pypilot tool has this connection ready-made set up; if you 'add' the connection in that tool, it adds a nmea client connection into signalk that pulls this data out of pypilot. This NMEA/TCP connection seems to land **only navigation.headingMagnetic** into the signalk's Data Browser. Although pitch and roll are available in the NMEA messages, it seems that signalk cannot process them because they are wrapped in XDR sentences. I think. Although openplotter has this facility to make this connection in signalk for you, you might just as well manually configure such a connection in SignalK yourself.

## Openplotter background script
The **openplotter background script** is an openplotter background process<sup>1</sup> that takes pitch and roll out of pypilot and puts it in signalk. The script is always running along with pypilot, but you don't see the data unless there is an signalk connection listening on 20220/UDP, and that is what the other 'connection' creates. This seems to be a workaround for getting pitch and heel into signalk, because the NMEA way does not work.

<p align="center"><img width="85%" src="https://user-images.githubusercontent.com/17980560/111814295-980a6c00-88da-11eb-8117-e65e2c1cde07.png"></p>

The openplotter script has a different function when you put the pypilot in the 'only compass' mode. In that mode, only the boatimu process is running, which means that there is no pypilot process that serves nmea on 20220/TCP. In this case, the openplotter script does add navigation.headingMagnetic, so here it comes into its own right.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/111814758-25e65700-88db-11eb-8168-cc852f00bc64.png"></p>

The data that comes from the openplotter background script identifies itself with the source 'OpenPlotter.I2C.pypilot', so if you see that in th signalk data browser, you know where it comes from:

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/111815225-b6bd3280-88db-11eb-9e79-d048f1441863.png"></p>

## Signalk zeroconf
The **modern** way of interfacing pypilot to signalk, is to have pypilot connect to signalk instead . This was introduced in March 2020 (v0.21) and it does require a recent version of pypilot; the tests here were done with v0.24. From a technical point of view, this method is exquisite: there is no senseless up-and-down sending of nmea messages to the whole world - no - there is one operational data store of nautical data, and as a client, you subscribe to updates of only the data you need. 

This signalk connection works with a zero configuration mechanism: pypilot will find the signalk server all by itself by issuing a 'service discovery request' onto the network. Some agent (more on that later) answers pypilot with the address of signalk, and then pypilot connects to signalk on that address. Pypilot will then issue an Access Request into signalk, and when you approve that request manually in signalk, a permanent connection is established. 

This requires some things to be set up, which are not there by default.

On openplotter, you need to install a few packages:
```
sudo pip3 install zeroconf
sudo apt install python3-zeroconf python3-websocket
```
How would you know that? If you run pypilot at the prompt and read the output line by line, it's there. This is the only way to troubleshoot this type of issues.

When you start pypilot, you will then see that pypilot is able to put out a service request for signalk:
```
signalk zeroconf service add myrouter._http._tcp.local. _http._tcp.local.
```
If there is no-one to answer that service request, nothing happens. The agent that should answer, is an **mdns server**: 'multicast dns service discovery'. This service is normally not available on your network, but it is provided by signalk, if you have Server->Settings->mdns checked. Remember you need to restart the SignalK server for this.

<p align="center"><img width="60%" src="https://user-images.githubusercontent.com/17980560/111764644-b5701380-88a3-11eb-96df-c2c02ef447ad.png"></p> 

 With mdns on, your pypilot server will find signalk:
```
signalk server found 10.10.10.1:3000
```
When pypilot connects to signalk (through the web service at 3000/signalk) it finds itself unauthorized, and therefor posts an access request (requestId).
```
signalk probe... 10.10.10.1:3000
signalk found ws://10.10.10.1:3000/signalk/v1/stream?subscribe=none
signak failed to connect Handshake status 401 Unauthorized
signalk post {'state': 'PENDING', 'requestId': '384b3471-9e76-401c-a663-c84f2fc30cad', 'statusCode': 202, 'href': '/signalk/v1/requests/384b3471-9e76-401c-a663-c84f2fc30cad', 'ip': '::ffff:10.10.10.1'}
signalk request access url http://10.10.10.1:3000/signalk/v1/requests/384b3471-9e76-401c-a663-c84f2fc30cad
signalk see if token is ready http://10.10.10.1:3000/signalk/v1/requests/384b3471-9e76-401c-a663-c84f2fc30cad {'state': 'PENDING', 'requestId': '384b3471-9e76-401c-a663-c84f2fc30cad', 'statusCode': 202, 'href': '/signalk/v1/requests/384b3471-9e76-401c-a663-c84f2fc30cad', 'ip': '::ffff:10.10.10.1'}
```
In signalK, you will see this access request showing up under the Security menu. When approving, you should set the permissions to Read/Write, otherwise pypilot still cannot write its measurements into signalk. Upon periodically polling ('see if token is ready'), pypilot will receive a security token and is able to make the connection:
```
signalk received token eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkZXZpY2UiOiJweXBpbG90LTg4NzQ2NDg3OTQ4IiwiaWF0IjoxNjE2MTQ2NTMzLCJleHAiOjE2NDc3MDQxMzN9.KSADIUWyEYKB9Go4_kltxidwb8fIj_u9EwI6mlyh_RI
signalk connected to ws://10.10.10.1:3000/signalk/v1/stream?subscribe=none
``` 
From that point on, you will also find navigation.attitude in your signalk data browser.

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/111764181-3b3f8f00-88a3-11eb-9848-ae39297a0768.png"></p>

***

This pypilot initiated signalk connection should be able to replace all nautical data interchange with pypilot: you should not need to send NMEA wind, track, gps data to pypilot anymore. When this connection works, port 20220 is obsolete.

So in short, to get signalk zeroconf working:
* install packages
* switch on mdns
* approve access request, read-write

***
> <sup>1</sup> For historic and completeness purposes, the openplotter background script resides at `/usr/lib/python3/dist-packages/openplotterPypilot/openplotterPypilotRead.py` and runs as a systemd service named `openplotter-pypilot-read`. The current version is incompatible with the latest version of pypilot, but the latest version of pypilot includes the signalk zeroconf client.

***
[Step 14: The Pypilot Motor Controller >>> ](Step-14-The-Pypilot-Motor-Controller)

* There is still an error in the log; I haven't figured out yet where this comes from but it does not seem to matter. 
* This might be related to the known issue is that pypilot connects to any other device that is advertised on mdns https://github.com/pypilot/pypilot/issues/89
```
Exception in thread zeroconf-ServiceBrowser__http._tcp.local._3028284512:
Traceback (most recent call last):
  File "/usr/lib/python3.7/threading.py", line 917, in _bootstrap_inner
    self.run()
  File "/home/pi/.local/lib/python3.7/site-packages/zeroconf/__init__.py", line 1759, in run
    state_change=service_type_state_change[1],
  File "/home/pi/.local/lib/python3.7/site-packages/zeroconf/__init__.py", line 1513, in fire
    h(**kwargs)
  File "/home/pi/.local/lib/python3.7/site-packages/zeroconf/__init__.py", line 1611, in on_change
    listener.add_service(*args)
  File "/usr/local/lib/python3.7/dist-packages/pypilot-0.24-py3.7-linux-armv7l.egg/pypilot/signalk.py", line 123, in add_service
    if properties['swname'] == 'signalk-server':
KeyError: 'swname'
```
