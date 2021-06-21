Motor.ino can be a daunting piece of software. I don't have classical understanding of it either. This is what I do know.

The mechanism that is at work is 'interrupt-driven pwm', and if you really want to understand the code, you have to google this first. It has to do with on-chip hardware timers that force the processor to interrupt whatever it's doing and jump to the code that has been defined as the 'interrupt handler'. You can spend your whole career doing software without running into it.

The output that it generates on the arduino data pins is quite generic and should cater for a range of motor drivers. The end of this page will list proven recipes based on the standard motor.ino. Rather than trying to tweak motor.ino, the pypilot builder is encouraged to use a proven driver that works with the standard motor.ino.

## Motor.ino Operating Modes
There are 3 operating modes for the type of output; in the code they are referred to as 'pwm_style':
* pwm_style == 0 "H-Bridge", selected by pulling D6 low
* pwm_style == 1 "RC-style", selected by pulling D6 high
* pwm_style == 2 "PWM-style", selected by uncommenting the line '//#define VNH2SP30 

**H-Bridge** - this operating mode allows the four outputs to 'directly' steer the gates of the four transistors in an h-bridge. D2 and D3 are the low side, D9 and D10 are the high side. I put 'directly' between brackets because there always needs to be some electronics in between the arduino and the transistor. What often happens in an h-bridge, is that the high or low side is always open during operation, and the (opposite) other side does the variable pulse-width (PWM). During the 'off' phase of the PWM signal, the motor's energy is allowed to circulate through the other side transistor.

This is how it looks like when the motor goes one way a bit and then stops again. You can see that there is a ramping-up phase, a steady phase, and a ramping down phase. During the ramp-up and ramp-down, the pwm-frequency is 15 kHz, during the steady phase, it's 65 Hz. The latter has to do with minimizing power dissipation, whilst still charging capacitors needed to drive the high-side mosfet gates. It's OK if you don't understand this yet, just look at the picture:

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/114552192-9c6d3f00-9c64-11eb-9eca-b6a267bad01b.png"></p>

There's something funny about the opposite low side: it also shows some pwm signal. I have no idea why this is, so if someone knows, please leave a comment.

If you want to build your own h-bridge, and you don't have formal education in electronics, be prepared to toast some parts. There are reference schematics on the pypilot.org site, and they are pretty robust, so if you think you know better, you'd better be sure.

If you want to use a commercially available h-bridge driver, pwm_style == 0 is often the mode to choose. D9 and D10 are the PWM signals for left and right, and D2 and D3 are the enable signals for left and right. The funny opposite high side signal is a bit weird here, and I haven't tried this myself yet.

**PWM-style** - This style is selected when you uncomment a line in motor.ino (see above). The output has a little higher level of abstraction: we have PWM, enable, and direction (2 times). So If you have a driver that has this type of inputs, this is the operating mode you need to select.

This operating mode has been a bit neglected in the past because of the availability of reliable drivers of this style. Since 2020, PWM-style is pretty much OK out of the box.

Drawback of this operating mode is still that it only operates at (audible) 1000 Hz pwm frequency. This results in your motor making a whistling noise. I thought it would irritate me, but I came to like it. 

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/114552387-d6d6dc00-9c64-11eb-9894-145df3162db0.png"></p>

**RC style** - This style apparently works with ESC drivers (electronic speed control). It seems there is whole world of people who know everything about it, but I'm not part of it, and I don't know what to google for and not getting inundated by aliexpress hits and the likes. It provides only one signal, which is a 50 Hz signal with variable pulse width. Apparently this is enough to drive an engine both left and right. I have yet to learn of the first person who has used it. If that's you, please make yourself known.

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/114552503-f5d56e00-9c64-11eb-84e6-5cc74f356967.png"></p>

## Proven combinations
Unless you really are into electronics, I would advice to purchase a ready-made one. This list is supposed to only show out-of-the box commercial drivers that work with out-of-the box motor.ino.

What | Who | Details
-- | -- | --
IBT-2 | [damien](https://forum.openmarine.net/showthread.php?tid=3388&pid=19024#pid19024) | pwm_style=2, D9 to R_EN and L_EN, D2, D3 to RPWM resp. LPWM
Pololu #2991: | ironman | pwm-style=2, D9 to PWM, D2 to DIR, D10 to <SPAN STYLE="text-decoration:overline;">SLP</SPAN>
LN298N | [kinefou](https://forum.openmarine.net/showthread.php?tid=2405&pid=12993#pid12993)
VNH5019 | CapnKernel

***

See also: [Demystifying motor.ino](https://youtu.be/Z4K_Hwsje40) _Youtube upload_
