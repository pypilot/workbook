While the boat is still on the hard, let’s have a look at tinypilot. Installation instructions for the image on https://pypilot.org – download are clear and concise and I have nothing to add. 

Note that it is mentioned here that the tinypilot won’t run on raspberry 3+ or 4. First feedback I read from people who tried to run it on a 3+ was indeed that it had a problem recognizing the wireless adaptor. The intended hardware is a raspberry pizero-WH. The W stands for wi-fi, the H stands for header pins, so you don’t need to solder.

```
pi@openplotter:~ $ wget https://pypilot.org/download.php?Down=images/tinypilot_2020_10_27.img.xz -O tinypilot_2020_10_27.img.xz
476.13M 5.69MB/s in 81s
pi@openplotter:~ $ xzcat tinypilot_2020_10_27.img.xz | sudo dd of=/dev/sda bs=4M
1.1 GB copied, 32 s, 32.8 MB/s
```

Once you have downloaded and burnt the SD card, stick it in and power it up. The pizero has a mini hdmi plug, so you could witness the startup sequence on a screen. But it won’t make you happy; we’ll get to that later.

Instead, like many IoT devices nowadays, Tinypilot manifests upon startup a wi-fi access point with a known SSID and wi-fi password, and you can connect your laptop to it to configure it. It is possible to keep the access point, and then tinypilot can serve as a very compact standalone system. For this exercise, however, we are going to connect tinypilot as a wi-fi client to our openplotter wi-fi access point, and access it from there. 

* After startup, you will see a wi-fi accesspoint called pypilot; it is open and does not require a wi-fi password. Connect to it, make sure your machine uses dhcp.
* Browse to http://192.168.14.1 and there you will see the browser interface of pypilot. 
* Go to Configuration → Configure Wifi
* Change Master(AP) to Managed(client), SSID to ‘openplotter’, and the wi-fi password of your openplotter access point, type in 10.10.10.2 for Client Mode Address, click submit.

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/111015975-a07b1800-83ab-11eb-9807-a6964a99fdd5.png"/></p>

* After submit, the screen will become unresponsive when the ip address was changed, because the ip address change will become almost immediately effective. So you'd need to change the address in the browser address bar if you want to continue with the browser interface.
* If you did not specify a fixed ip address in the Client Mode Address box, find the ip address of your tinypilot by typing the following on your openplotter:
```
pi@openplotter:~ $ sudo grep DHCPACK /var/log/daemon.log | tail
Mar 10 22:26:46 openplotter dnsmasq-dhcp[579]: DHCPACK(wlan9) 10.10.10.159 a4:34:d9:e4:bc:d6 marco-desktop
Mar 10 22:46:32 openplotter dnsmasq-dhcp[579]: DHCPACK(wlan9) 10.10.10.82 b8:27:eb:16:28:c9 box
```
In the example above, you see that the last IP-address that was handed out, was 10.10.10.82, so you should be able to type http://10.10.10.82 in your browser to get back to the browser interface of tinypilot. 

> If you did specify a fixed IP address in the boc Client Mode Address, as shown in the picture, you don't have to find out what your IP address is, because you already know. The address you specify must be between 10.10.10.2 and 10.10.10.49, otherwise there might be clashes with addresses that openplotter has handed out to other clients.

You can now go to the OpenCPN plugin on your openplotter machine. When you change the ip address in Config --> Host to the IP address of your tinypilot, your OpenCPN plugin is now controlling your tinypilot.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110994961-e0290c00-8379-11eb-967b-e21ff14f32dd.png"/></p>



***
[Step 11 Tinypilot under the hood >>>](Step-11-Tinypilot-under-the-hood)