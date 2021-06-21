Pypilot is either ‘Standby’, or ‘On’. If it is ‘On’, there are four different autopilot modes: Compass, GPS, Wind, True Wind. Let’s put them in a list:
* Standby
* Compass
* GPS
* Wind
* True Wind

We'll go through each of these basic functions.

## Standby mode
In this mode, each user interface will show the **current compass heading**. Mind you, this is the compass heading that comes from the pypilot IMU. If you have another electronic compass on your boat, pypilot will totally ignore it. 

Next to showing the compass heading, there are some buttons to **move the rudder** left or right or possibly center it. This is intended to align your actuator with your tiller so you can hook it on. And, if you are lazy you could steer the boat with your buttons. 

Then, there is list of **autopilot modes** you can choose from. The compass mode is default and always available. The GPS, Wind and True Wind options are only available under certain conditions, which I will explain below. When you have chosen an autopilot mode, you can hit the AP (autopilot) button and then you enter the chosen autopilot mode.

## Compass Mode
In compass mode, the boat will steer the commanded heading. When you enter compass mode, the  commanded heading defaults to the current heading, but you can change the commanded heading with the <<, <, >, >> buttons. I’m explaining here in a difficult way what everybody knows an autopilot does, but I’m introducing some terminology here, so bear with me.

It is as simple as that, but at this point two things are important to mention. 

First, the pypilot **compass heading**. As said, this is the compass heading that is determined by pypilot from the readings of the IMU. A compass heading is a 1-dimensional value, so why would you need a 9-axis IMU to determine it? In short, any boat distorts the magnetic field, not only differently for different headings, but also for different pitch and roll attitudes. Any sailor knows about deviation tables, but for driving an autopilot, you would also need to know the deviation for various pitch and roll attitudes. Pypilot constantly determines those deviation values based on the IMU in a process called autocalibration. However, some basic, initial calibration is still needed. The result is a very steady compass reference to steer by.

Second thing to mention is the **gains** of the steering mechanism. They determine reactiveness of the steering, and suppression of overshoot. There are quite a few gains available. The values of these gains are different for each boat, so they need to be set properly. 

So to use compass mode, you need to make sure your compass has been calibrated, and your gains are set properly. More about both of them later.

## GPS Mode
Another autopilot mode is the GPS mode. In fact, this is a bad name. It should have been called Track mode. Because that is what it does. In this mode, pypilot follows a track.

A track is defined by a _bearing_ (intended heading) and _cross track error_. If pypilot is set in Track mode, it will steer the bearing, but correct the bearing based on the cross track error. For instance, if the bearing is due north, but you are 50 meters left of track, pypilot would steer 010 to get back to track.

Plotter software like opencpn will send out track information for subsequent segments of a route. If you create a route in opencpn and activate the route, pypilot will pick it up and enable the Track mode, so you can select it. There is some debate about whether pypilot should be forced to follow a track when activated, or allow you to choose compass mode nevertheless. Also, when you pass a waypoint, pypilot will flick to the new bearing without warning. So some care must be taken using the Track mode.

## Wind Mode
You might have a wind unit on your mast that gives you the direction the wind comes from, relative to the bow. If this so-called _wind angle_ is not corrected for your own speed, it is the _apparent wind angle_. There are reasons to steer by apparent wind angle, to keep the apparent wind angle constant. If you are sailing close-hauled, for example, you might want to benefit from wind shifts or at least prevent your boat from stalling when the wind shifts towards the bow. 

Pypilot Wind mode (better: Apparent Wind mode) is enabled when pypilot receives apparent wind angle information. If selected, it will steer a compass heading that will maintain a constant average wind angle, and adjust that heading when the average wind angle shifts. So, you still need an IMU when sailing by wind.

## True Wind mode
A disadvantage of sailing by apparent wind, is that the apparent wind angle is dependent on boat speed and wind speed: the faster you sail, the more the apparent wind shifts towards the bow. In cases where your own boat speed varies, for instance if you have a following sea and are surfing off waves, or if the wind is very gusty, it might be better to sail by the true wind angle. To determine true wind, you need to know both the boat speed and the wind speed. The wind speed you probably get from your wind set, and pypilot only uses SOG for the boat speed, and expects this from a GPS message. Therefore, pypilot will enable True Wind mode when it receives both wind and SOG information.


***

So those are the primary functions of pypilot in nutshell. For these basic functions to work, you need to have 

* your IMU sensors calibrated; 
* your gains properly set up; and 
* you need to provide pypilot with the right nautical data. 

These secondary functions are up next.

## IMU Sensor Calibration
When an IMU is installed on a boat, it does not know its position. There is no set way to position it: you can put it in any way you want, as long as it is free of sources of magnetic interference. Therefore, you have to tell pypilot when the boat is level. All user interfaces have a facility for that; it is a button called ‘boat is level’. Do not forget to do this. If you don’t level it, there is no error message but the pypilot will steer like a goat.

Once pypilot knows which way is up, it will recognize the boat’s turning, and it will start correlating the turning motion to the measurements from the internal compass, thereby building its internal 2D deviation table. Pypilot needs little more than a 180 degree turn to finish its initial compass calibration, and signals this with resetting its calibration age. Normally, this happens all by itself. Thing to remember is that at first pypilot might be a bit nervous, but quite quickly comes to its senses, so if you feel a strong urge to throw it overboard, just hold on to it for a little longer.

From then on, the 2D compass calibration will continue to take place. There is also a 3D compass calibration, that builds this deviation table for various pitch and heel positions. The progress of this autocalibration mechanism is shown with all colours in a 3D globe representation, but that’s all string theory to me.

## Gains
We will finish this at a later point in time.

***
[Data connections >>>](Data-connections)