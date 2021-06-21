So we have software, and we have hardware. How do we interact with this system? We do that through a _user interface_. Before we describe the various options, first a word about the internal workings of the pypilot software.

The pypilot software consists of various modules that run at the same time in parallel. Software can do that. The modules interact with each other. How do they interact? They interact by reading and writing things on a common data storage module, like a blackboard. For instance, the compass module writes the current heading to the blackboard, the user interface module writes the intended bearing to the blackboard, and then the autopilot module reads those two values from the blackboard and steers the boat.

Now it becomes clear how a user interface could work: It shows on a display the values of some of those blackboard parameters, and if you press a button, the user interface writes new values to that blackboard. Then, the combined hardware and software system does its thing based on what it reads from the blackboard.

In fact, that is how the pypilot user interfaces all work. There are a couple of them. They all write and read to this same blackboard. They can even work in parallel: if you press a button in one user interface, you see the same thing happening in the other user interface. Roughly spoken, all these interfaces offer the same functionality.

Now let’s go through the various user interfaces that are available:
* Browser interface
* Hat Interface
* OpenPlotter Interface (Python)
* OpenCPN Plugin Interface

First, the **Browser interface**. When you start pypilot, you can go to http://your-pypilot-server and you get a web based interface with all basic pypilot functions. What those functions are, I’ll explain later. It should always work from any laptop, tablet or smart phone. Of course, sailing a boat with a phone in your hand is not optimal, but it’s a good last resort to keep in mind.

Second, the **Hat Interface**. This is the one that works with the small LCD screen and little keyboard. It was baptized Hat because it typically works with some hardware circuit board that you can stick on top of the raspberry. You don’t really need this specific hardware circuit board: you can just wire your keys straight to the gpio pins of the raspberry. The Hat interface offers the full set of pypilot functionality in menu items on the LCD panel interface. In combination with the light-weight TinyPilot distribution image and a miniature raspberry (“pizero”) this makes for an autonomous, very compact pypilot implementation.

Third, the **Openplotter Interface**. As said, you can download the OpenPlotter marine navigation system and it has pypilot included. Specially for OpenPlotter, a python-based interface was added, with popup windows etc. This Openplotter interface is typically connected to the same pypilot that runs on the openplotter raspberry. Again, it has all the standard functionality that pypilot offers. 

Last, but not least, there is a very special pypilot interface, that takes the shape of a **plugin to OpenCPN**. OpenCPN is nautical chart plotter software, it has a moving map etc. If you install the pypilot plugin in opencpn, you get an extra button, and when you click it, yet another pypilot user interface pops up. In theory, this plugin has nothing to do with opencpn, in the sense that it is yet another pypilot user interface. It is relatively slick and snappy because it has been programmed in C++, like OpenCPN, and it’s conveniently located under a button, so it does not take long to start up. What makes it special is that is has some specific integration features with OpenCPN. For instance, it shows on the map where pypilot is steering towards. More on this later.

***

So these four user interfaces all have in common that they interface to the pypilot system through this blackboard module, that is part of the pypilot software. The real pypilot functions are defined by the pypilot software; the user interfaces only make them available to you. So now it is time to describe what those pypilot functions really are.

***
[Pypilot functions >>>](Pypilot-functions)