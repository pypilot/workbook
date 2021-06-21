As said, the pypilot software component is not complete without the motor controller software, and this has to run on an Arduino. As far as I know always an Arduino Nano is used, the small version.

Try not to buy a cheap clone. There are some settings on an Arduino that require ugly tricks to set right. These settings are called fuses, and I compare them with BIOS settings on a PC. If you buy a proper Arduino, the fuses are set all right. If you purchase them from a major retailer, this will increase your chances to get a proper Arduino. If your fuses are wrong, pypilot will give you an clear error message saying your fuses are wrong.

Next is to get the sketch. You need two files and you best get them from github: https://github.com/pypilot/pypilot/tree/master/arduino/motor. The files are motor.ino and crc.h. 

To get the sketch onto the Arduino, the easiest way is to do it with a USB cable. There are two options: you program the Arduino the Linux way, or the Windows way.

* The Linux way is explained in the github readme. It requires a program called avrdude that is not on the openplotter image, so you would have to install it with `sudo apt-get install avrdude`. Don't forget `sudo apt install arduino` as well.
* The windows way requires the download and installation of the [Arduino IDE](https://www.arduino.cc/en/software/). Put the two files in a directory called ‘motor’, double-click on motor.ino, in the Arduino IDE Tools menu set Board=Arduino Nano, Processor=ATMega328P, set the com-port, and hit Upload (CTRL U). 

When the tool says 'upload completed', the next step is to connect your arduino to the pypilot. The easiest way to hook up a Nano to your pypilot is to use the same USB cable that you programmed it with. Like this:
 
<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110981993-44db6b00-8368-11eb-8876-92fadeb8c170.png"/></p>

The USB-attached Nano will enumerate as a serial device and Pypilot will probe all serial interfaces periodically, meaning that after a while, the pypilot will automatically recognize the presence of the Arduino. When this happens, the first thing you should notice, is that the Tx and Rx LED’s on the Nano will start blinking. This means that pypilot is talking to the Nano. In the autopilot control, you will see top-left the word Arduino appear, in the middle the word SYNC, and sometimes BADVOLTAGE_FAULT. These are the tell-tale signs that a brilliant system is starting to become alive. Judging by the word FAULT, we’re not there yet, but if you got this far, you really deserve a proper drink.
 
<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110982106-66d4ed80-8368-11eb-999b-38d11beb0e5f.png"/></p>

We are doing this step just to recognize what functionality starts working at what stage. At this stage you should see the following:

* Both Tx and Rx should blink, meaning that there is two-way traffic between pypilot and the Nano. With a USB connection they both blink or they both don’t. When you connected another way it might be that Rx blinks and Tx does not. If that is the case, give a shout.
* If you enable the autopilot (‘AP’) you should see the word ‘Engaged’ in the autopilot control, and on the Arduino another red LED should burn.
* There will be random error messages next to the word SYNC, and this can be expected. These error messages are called ‘flags’ in pypilot parlour, and at this point they are caused by the input pins on the Arduino being left open. Open input pins are bad practice in electronics; if they are left 'floating' they are susceptible to electrostatic influences and worse, damage.
***
[Step 7: OpenCPN plugin >>>](Step-7-OpenCPN-Pypilot-Plugin)