We left the Arduino dangling there on the desk, with all input pins at the mercy of electrostatic discharges. So the next thing that needs to be done now is to wire up the Arduino, basically: build the electronics. Everything you need to know is in the comments in the source code of motor.ino. 

The **reference schematic** can be found on the pypilot store, on the page for Pypilot Motor Controller, scroll down and click [schematics](https://pypilot.org/schematics/hbridge_controller.pdf). The first page is an H-bridge motor driver implementation, the second page shows how to wire all inputs. 

The reference schematic is a robust design and will survive abuse. Many people decide not to stick to this design for various reasons:
* Problematic availability of some parts in this design. 
* Easy availability of all other kind of motor controllers
* Different insights
* Different requirements

First a few thoughts about the design.

The Arduino and motor controller being separated from the pypilot raspberry has its roots in separation of high PWM currents from the delicate IMU. The reference schematic features a **serial connection** with galvanic isolation. This is for various reasons:
* The connection is prone to picking up interference and should be properly protected against EMS.
* Also, because the 5V logical level from the Arduino does not match the 3V3 logical level of the Raspberry
* You don’t want to have a ground loop here, given high motor currents, lengthy cables, high-frequency PWM signals.

Short distances and bad availability of galvanic decouplers seem to have moved implementers into skipping the galvanic isolation, running into the need to use level shifters instead. USB connection between the two is used in prototypes, but not advised for operational setups.

The Arduino has 3 so-called PWM styles, which are really motor driver styles.
•	Style 0 signifies an H-bridge, as in the reference schematic. Four output pins of the Arduino correspond to 4 FETS in an H-Bridge. Style 0 is selected by tying pin D6 of the Arduino to ground.
•	Style 1 signifies an RC-style signal, a 50Hz signal on D9 where the pulse-width dictates the requested position of the servo. RC airplanes work like this. To select style 1, tie D6 to VCC. This style also mentions programming of an ESC, but I have no idea how this works. Input very welcome. 
•	Style 2 gives you a pwm (D9) and a direction (D2 or D3) signal. This style is a bit of the runt of the litter, as it did not receive much love in its infancy, until recently. You have to enable it by editing motor.ino and uncommenting #define VNH2SP30. Enabling this style has messed up sensor readings in the past, so make sure you have the latest motor.ino. 

I’ve analyzed the signals that came out of it in the past; see this clip: https://youtu.be/Z4K_Hwsje40

Sensors related to Arduino pins include controller temperature, motor temperature, motor current, driver voltage, rudder angle feedback, end-of-travel switches. Some measurements from these sensors are processed in the Arduino (end-of-travel, current, voltage), others are sent back to the pypilot software and used for servo operation and statistics gathering. If you don’t implement a sensor, you should tie the input to ground and disable the sensor in the motor.ino code. It’s better to implement all sensors, because most of them are there for safety.

In the ‘basic’ and ‘simple’ pilot steering algorithms, the rudder feedback is not used for positioning the servo, only for soft end of travel stops. In the recently released pilot steering algorithm ‘absolute’ the rudder feedback is used for positioning.

For now this ends this little braindump on the electronics. If you are not into electronics, you’d better buy a ready-made Pypilot Motor Controller. 

***
[Step 10: Installing Tinypilot >>>](Step-10-Installing-Tinypilot)