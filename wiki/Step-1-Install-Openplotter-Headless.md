First thing we do is to install openplotter, because it is by far the easiest and quickest way to get pypilot up and running. 

To install openplotter, just follow the instructions on [this link](https://openplotter.readthedocs.io/en/latest/getting_started/installing.html): Installing / Basic. You only need an SD card, a Raspberry Pi, and an SD card reader. ‘Headless’ means that you don’t need to hook up any screen, mouse or keyboard to your raspberry: you access the raspberry through VNC, a remote desktop type of thing.
* **Download** ‘Openplotter Headless’ from [this link](https://openplotter.readthedocs.io/en/latest/getting_started/downloading.html), and unzip it. I got an image file called 2020-12-16-OpenPlotter-v2-Headless.img after unzipping.
* **Burn** that file onto the SD card with the [Raspberry Pi Imager](https://www.raspberrypi.org/software/). In the Operating System dropdown box, scroll all the way down, 'Use Custom', then select the .img file.
* When the SD card has been written, take it out of the SD card reader, **stick** it in the Raspberry, and plug the power cord in the raspberry.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110975789-7819fc00-8360-11eb-8c05-6d49a67040b3.png"/></p>

* After no more than 2 minutes, you will see a Wi-Fi access point appear, called ‘openplotter’. Connect your computer to it. The Wi-Fi password is 12345678.
* Windows has a bad habit of dropping Wi-Fi connections when it cannot ping Bill Gates. So keep an eye on your connection, you might have to reconnect a few times.
* If it sticks, you will get an IP address in the 10.10.10.0 range and you can ping 10.10.10.1. That is your openplotter raspberry.
* Now download RealVNC VNC Viewer from [here](https://www.realvnc.com/en/connect/download/viewer/windows/). Open it and connect to 10.10.10.1:5900. The username is pi, the password is raspberry. You will see the desktop and there will be a warning. If you are on Linux, you can use Remmina (ubuntu) or another VNC client.
* Ok the warning, and hit Next a few times. Along the way you need to give a new password for the user pi. At the end, this is what you see:

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110821795-b516bd80-8290-11eb-9b91-c281b9c4ad36.png"/></p>

* Don’t forget to treat yourself to something nice! You’ve achieved the first step.

***
[Step 2: Install pypilot >>>](Step-2-Install-pypilot)