So, you either install pypilot from source, or you download a distribution. What more do you need? 
There are two specific hardware components that are always needed for pypilot:
* The Inertial Motion Unit (IMU); and
* The Arduino Controller.

The **IMU** measures _compass heading_ and the _motion_ of the boat. As we will see later, these two (heading and motion) are inextricably linked: you cannot separate them and they both are crucial to the pypilot functioning as an autopilot. The IMU is one single chip (integrated circuit) and it needs to be connected to the raspberry pi. Typically, it sits on top of the raspberry gpio header. Pypilot cannot function without this IMU – for example, it is not possible for pypilot to steer based on an external compass.

The **Arduino Controller** is the link between the pypilot software and the actuator (motor) and other sensors. It is always an Arduino, and typically an Arduino Nano. The software that must run on this Arduino (in Arduino-parlour: a _sketch_) is included in the pypilot software distribution, and this sketch is called motor.ino. You must take this file from a pypilot directory and burn it onto the Arduino with Arduino programming software. 

The Arduino Controller talks to the pypilot raspberry through a _bidirectional serial connection_. This can either be a serial TTL connection or a USB cable. The protocol that is spoken over this connection is binary, bespoke, and highly optimized. It makes no sense trying to interface at this level, and nobody has ever tried. In one direction, pypilot tells the Arduino controller to move the rudder, how far and how fast. In the other direction, the Arduino controller tells the pypilot various things about the measurements of the sensors that are connected to the Arduino, like rudder angle, and the condition of the controller itself, like voltage, current and temperature.

The Arduino Controller is not a motor controller, but it connects to various types of motor controller. This can be an H-bridge interface, an RC style interface or a PWM controller. Details on how to select the type of motor controller are documented in the code of the motor.ino sketch itself, and they can be set by pulling Arduino pins high or low. Occasionally, you might have to tweak the sketch.
 
On the pypilot.org web site and in common conversation, the Arduino controller with an H-bridge motor controller attached to it, is called the ‘Pypilot Motor Controller’, and you can purchase one in the store. This might confuse people into thinking that you can take any other common motor controller instead and hook it directly to the raspberry. You cannot, because then you miss the Arduino Controller with its specific sketch. You could say that pypilot software is not complete without the Arduino sketch.

![image](https://user-images.githubusercontent.com/17980560/110975538-2b362580-8360-11eb-83a6-9a88921e6822.png)


***
[The User Interface component >>>](The-User-Interface-component)