Let’s have a look at a few parts of the user interface that are relevant for now. Click the ‘Control’ button. The **Autopilot Control screen** comes up. You will have this:

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110976103-c7f8c300-8360-11eb-9905-306d79e21ec2.png"/></p>

* The **Autopilot Control screen** is the equivalent of the physical button panel known from boat autopilots. You will see a compass heading. Turn your raspberry and you will see it moving. The big red AP is the button that turns the autopilot on. For now, only Compass mode is enabled. We are not feeding it track or wind data yet, so that makes sense. 
* Both Autopilot Control and the pypilot User Interface app have a button **Calibration**, which points to the same facility. The Calibration facility has 5 tabs, of which the first is Alignment. Here you will see that very important button ‘Boat is level’. Click it and the boat representation will righten. The rest of the calibration I never got to work at home. It only seems to work on the boat. Don’t ask me why.
* The **Client** button brings you straight into that pypilotServer blackboard database. You can see the whole content of that database, and many items you can change. This is deadly dangerous, because when you scroll through the list, sometimes the mouse roll button hits a slider and you are instantly changing values. There is no cancel button. I treat this screen with utmost respect: we have not made any backups yet of the (settings) database.
* The **Scope** facility is a lot more forgiving to play around with, but it’s not interesting at home, only on the boat. The ap.heading_error is an interesting one if you are tuning your gains. But we are not there yet!
* Coming back to the Autopilot Control, there are a few more things that need explanation.
  * The top left ‘none’ signifies there was no Arduino controller found.
  * ‘Disengaged’ means the actuator (motor) is currently not working the rudder.
  * The ‘Pilot’ dropdown shows alternative steering algorithms. Default is basic, you can choose simple as well. Learning is a self-learning algorithm but it is experimental.
  * The horizontal slider would steer the rudder directly.
  * All vertical sliders with numbers and letters are the individual gains of the chosen steering algorithm. The colorization is a feedback of the individual components of the steering algorithm doing their best to steer the boat. If you turn the IMU, you will see some activity here.

We have gotten our bearings around the first pypilot User Interface. This was the openplotter one. Almost all functions that we have seen here are also available in other user interfaces.

***
[Step 4: The browser interface >>>](Step-4-The-browser-interface)