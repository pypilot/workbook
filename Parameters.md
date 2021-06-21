The following list of parameters are limited to those that are saved in pypilot.conf.

| ap.mode | default: compass; screen: Control
 -- | :-- 
| | Chosen autopilot mode (e.g. compass, gpc, wind, true wind); apparently this choice survives a restart.


| ap.pilot | default:  basic; screen: Gain screen
 -- | :-- 
| | Current choice of autopilot algorithm.


| ap.pilot.* | |
 -- | :-- 
| | These are the gains for the different autopilots. See [Gains](gains)

**Tacking functionality**<br>
Basic idea when you hit tack <br>https://forum.openmarine.net/showthread.php?tid=1469&pid=7184#pid7184:
1. wait **tack delay**
2. change course to **tack angle** or opposite wind angle
3. move rudder at **tack rate** until it hits end of travel, or boat is **tack threshold**

| ap.tack.angle | default: 100; screen: Configuration screen
 -- | :-- 
| | Degrees. In wind mode it is automatically determined from current course <br>https://forum.openmarine.net/showthread.php?tid=1469&pid=7184#pid7184


| ap.tack.count | default: 0; screen: 
 -- | :-- 
| | Number of tacks made.


| ap.tack.delay | default: 0; screen: Configuration screen
 -- | :-- 
| | How many seconds to wait to tack after hitting tack button <br>https://forum.openmarine.net/showthread.php?tid=1469&pid=7184#pid7184


| ap.tack.rate | default: 20; screen: Configuration screen
 -- | :-- 
| | How quickly to tack. Unit is degrees/sec. <br>https://forum.openmarine.net/showthread.php?tid=1469&pid=7184#pid7184


| ap.tack.threshold | default: 50; screen: Configuration screen
 -- | :-- 
| | When to revert back to normal filter. Unit is percentage. Typically half the tack angle but adjusting this would be useful to prevent overshoot. <br>https://forum.openmarine.net/showthread.php?tid=1469&pid=7184#pid7184


| apb.xte.gain | default: 300; screen: Client
 -- | :-- 
| | In GPS mode (when following an APB track), correct the track heading based on the XTE, multiplied by this gain. Example: track is north, but XTE is 0.05 left. Pypilot will steer 000 + 300 * 0.05 = 015 to go back to the track.<br>https://forum.openmarine.net/showthread.php?tid=2078&pid=10805#pid10805


| imu.accel.calibration | default: ; screen: 
 -- | :-- 
| |   


| imu.accel.calibration.age | default: - ; screen: 
 -- | :-- 
| |  


| imu.accel.calibration.locked | default:  false; screen: Client
 -- | :-- 
| |  


| imu.accel.calibration.points	 | default: ; screen: 
 -- | :-- 
| |  


| imu.alignmentQ | default: ; screen: 
 -- | :-- 
| | This seems to be the result of the Boat is Level action, and it is a vector the points to the sky. 


| imu.compass.calibration | default: ; screen: 
 -- | :-- 
| |  


| imu.compass.calibration.age | default: - ; screen: Calibration - Compass
 -- | :-- 
| | When this resets to 0, you know your compass just calibrated. I check this at the start of a trip, do a semi circle, wait until it resets.


| imu.compass.calibration.locked | default:  false; screen: Calibration - Compass
 -- | :-- 
| | Lock compass calibration, when you don't want any surprises.


| imu.gyrobias | default:  false; screen: 
 -- | :-- 
| | 


| imu.heading_offset | default: 0; screen: Calibration screen
 -- | :-- 
| | When you installed your IMU in your boat, it did not have any reference to where north is. After telling pypilot that "Boat is level", this corrects the compass heading. Tweak it until the compass heading in the pypilot control screen equals the corrected heading read from your boat compass, or, with no current or drift, your COG.


| imu.rate | default: 10; screen: Client
 -- | :-- 
| | Can be set to 10 or 20; apparently this has to do with update rate from the compass/imu. Unit is Hz: number of updates per second.<br>https://forum.openmarine.net/showthread.php?tid=1849&pid=9756#pid9756



| nmea.client | default: ; screen: 
 -- | :-- 
| | 

These setting result from the rudder calibration procedure https://pypilot.org/wiki/doku.php?id=calibration, in case you have a rudder feedback sensor. I'll leave it here because this might become topical with the advent of the 'absolute' pypilot algorithm.

| rudder.nonlinearity | default: 0; screen: 
 -- | :-- 
| | 


| rudder.offset | default: 0; screen: 
 -- | :-- 
| | 


| rudder.range | default: 45; screen: Calibration screen
 -- | :-- 
| |


| rudder.scale | default: 100; screen: 
 -- | :-- 
| | 


| servo.amp_hours | default: 0; screen: Statistics
 -- | :-- 
| | Collective amp hours that the pypilot has used. Can be reset with the button Reset. Could be used for monitoring or optimising pypilot efficiency.


| servo.compensate_current | default:  false; screen: Client
 -- | :-- 
| | No references, does not seem to be used in the code.


| servo.compensate_voltage | default:  false; screen: Client
 -- | :-- 
| | According to the code of servo.py, this compensates the motor speed for fluctuating battery voltage (!).


| servo.current.factor | default: 1; screen: Client
 -- | :-- 
| | You can fine tune the current reading with this one. Tricky thing to do - use pypilot_scope to plot servo.current, and be very quick reading an amp meter in the power supply line of pypilot while moving the motor; then calculate a factor.


| servo.current.offset | default: 0; screen: Client
 -- | :-- 
| | See servo.current.factor. Offset, what can I say.


| servo.disengage_on_timeout | default:  true; screen: 
 -- | :-- 
| | No references on forum.


| servo.faults | default: 0; screen: 
 -- | :-- 
| | Not visible in any screen, but apparently it counts the servo faults. That's a diagnostic 


| servo.gain | default: 1; screen: Client
 -- | :-- 
| | Effectively an overall gain, although it has to do with the servo speed. It defaults to 1, so changing to 2 or 3 will get much more response. The servo gain was added for extremely fast/slow motors and also a way to change wires.<br>https://forum.openmarine.net/showthread.php?tid=1924&pid=9789#pid9789<br>https://forum.openmarine.net/showthread.php?tid=2657&pid=14394#pid14394



| servo.max_controller_temp | default: 60; screen: Client
 -- | :-- 
| | In degrees C, when the NTC measures a temperature higher than this, it shuts down pypilot with an OVERTEMP servo flag. Saved me once, when I fried my H-Bridge.


| servo.max_current | default: 7; screen: Configuration screen
 -- | :-- 
| | If the current used by the motor exceeds the max_current, it will stop the motor and raise a FWD_FAULT or REV_FAULT flag. This also works as a rudimentary end-of-travel stall mechanism. If max_current is set too high, the overcurrent will not trigger if the motor stalls. If too low, it will trigger by mistake. Pypilot temporarily increases this value to get an engine 'unstuck' when it has reached the end of travel.<br>https://forum.openmarine.net/showthread.php?tid=3361&pid=18829#pid18829<br>https://forum.openmarine.net/showthread.php?tid=2078&pid=19068#pid19068


| servo.max_motor_temp | default: 60; screen: Client
 -- | :-- 
| | See servo.max_controller_temp. 


| servo.max_slew_speed | default: 18; screen: Configuration screen
 -- | :-- 
| | acceleration ( setting lower will reduce current spikes but also responsiveness, this setting can affect the servo.max_current setting)<br>https://forum.openmarine.net/showthread.php?tid=3361&pid=18829#pid18829


| servo.max_slew_slow | default: 28; screen: Configuration screen
 -- | :-- 
| | deceleration, see servo.max_slew_speed


| servo.period | default: 0.4; screen: Configuration screen
 -- | :-- 
| | the minimum time the motor is moving in a certain direction or stopped, reducing will make shorter movements with less reaction time, but increasing will reduce the wear on the motor and avoid as many counter corrections.<br>https://forum.openmarine.net/showthread.php?tid=3361&pid=18829#pid18829



| servo.position.{d,i,p} | default: 0.02; screen: Client
 -- | :-- 
| | No references found on the forum. From the naming and code in servo.py it can be deduced that it is related to the PID control mechanism that brings the rudder into a certain position. I'll leave it here because this might become topical with the advent of the 'absolute' pypilot algorithm.


| servo.speed.max | default: 100; screen: Configuration screen
 -- | :-- 
| | the maximum movement speed, usually always 100. If you had a 24 volt system and motor controller and 12 volt motor you could use 50 to avoid burning it out <br>https://forum.openmarine.net/showthread.php?tid=3361&pid=18829#pid18829


| servo.speed.min | default: 100; screen: Configuration screen
 -- | :-- 
| | the minimum continuous speed. This can also generally be set to 100 to get the best performance. As it is reduced the noise of the ram decreases but generally at the expense of power consumption, especially on high friction drives. If this is too low, the efficiency is worse. How much worse depends on the type of motor and drive. For lead screws use 80-100, for more efficient drives like ball screws you can go down to 50. <br>https://forum.openmarine.net/showthread.php?tid=3361&pid=18829#pid18829


| servo.use_eeprom | default:  true; screen: 
 -- | :-- 
| | Some servo settings, like rudder calibration, slew speeds, max current and a few others are stored on the motor controller's eeprom. This way the motor specific settings are not lost if you upgrade, and if you have two motor controllers and two motors you could use the backup one with different settings. Disables reading the values from the motor controller, if you change it to "False". If you do this and e.g. set the slew values in the config, they will be used and the motor controller will not be read. You might try this if you have trouble with the slew getting reverted. <br>https://forum.openmarine.net/showthread.php?tid=3042&pid=16726#pid16726<br>https://forum.openmarine.net/showthread.php?tid=2212&pid=11625#pid11625


| servo.voltage.factor | default: 1; screen: Client
 -- | :-- 
| | This can be used to fine tune the voltage reading in the Statistics screen. If your controller voltage is beyond certain values, you will get a BAD_VOLTAGE server flag and your pypilot will not work. If the voltage is really way off, you have a bad configuration of your voltage divider resistors so you need to go back to the soldering iron.


| servo.voltage.offset | default: 0; screen: Client
 -- | :-- 
| | See above: servo.voltage.factor


| wind.offset | default: 0; screen: Configuration screen
 -- | :-- 
| | No references found on the forum. 


