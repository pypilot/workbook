This is for when you ordered a ready-made pypilot controller and it arrived. The question is: what next.

This is a 2019 model:

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/111851755-d0c73700-8914-11eb-9e09-b276ef2d50cb.png"></p>

This is how a 2021 model seems to looks like:

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/111852080-f1dc5780-8915-11eb-87bc-e001f3aceaf1.png"></p>

The 2019 model matches exactly the schematics on the pypilot site. 

As you can see on the first picture, when you connect 12V to the motor controller, and you don't connect it to the raspberry, there is one led that will light up on the arduino. The colour might be green or red. This is the power led.

The 4 wire cable with a watertight connector should be connected to the raspberry as follows:

 Pypilot motor controller |  Raspberry pi
-- | -- 
Vcc  | 3V3 pin 1  
RxD  | TxD pin 10
TxD  | RxD pin 8
Gnd  | Gnd pin 6

Please note that RxD and TxD are crossed. This is a point-to-point connection, and for that you connect TxD to RxD and vice versa. I won't mention wire colours here, you must figure that out yourself. If you bought an IMU from the pypilot store it comes with a cable that matches the one from the pypilot motor controller.

Once hooked up to the raspberry, it is very likely that the RxD led on the arduino will burn continuously, in addition to the power led. This means that the UART has not been set up on your raspberry. 

To fix this, click Raspberry -> Preferences -> Raspberry Pi Configuration, click the Interfaces tab, check Serial Port, Uncheck Serial Console, Click OK, don't reboot yet.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/111851375-2f8bb100-8913-11eb-9cb0-82ec5b7084d2.png"></p>

Then, go to Raspberry->Openplotter->Serial, and click on UART. It will turn grey and beg for a reboot:

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/111850508-c7d46680-8910-11eb-94ab-4915599029fc.png"></p>

After the reboot the RxD light on the arduino should be off. Then you can run pypilot on the console and check the output. You should see that it finds an arduino controller on the /dev/ttyAMA0 interface. At that point, the Rxd and Txd lights on the arduino should blink very quickly, like 4 blinks per second. All your pypilot user interfaces should indicate the presence of a motor controller.

 * [Step 15: Understanding motor.ino](Step-15-Understanding-motor.ino)