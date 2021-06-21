In this step, you will install pypilot. In Openplotter, this is dead easy. Referring to the story above, this will install the pypilot software component, and the openplotter User Interface component.

* The top left corner shows a raspberry icon. This is your start menu. When I say Raspberry, I mean this icon.
* Click Raspberry&#8594;Openplotter&#8594;Settings. In the first tab, you will see a list of Openplotter apps. You will see ‘Press refresh’ at least 7 times in your screen, so don’t ignore it. It is this button:

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/110821208-25710f00-8290-11eb-80f0-70bf8b9ef610.png"/></p>

* At this point, the Settings panel will hang. It hangs because it is trying to access the internet. You can now try to act like a smart IT professional, trying something for which you don’t have to get off your chair, or you can get yourself a good old ethernet cable and stick it in the raspberry. For the sake of getting on with it, I suggest the latter. The screen will refresh with version numbers.
* Scroll down to Pypilot, click it, install it, say yes. It will take a few minutes. You will have ample time to get yourself a cup of tea.
When it’s done, you will see a new menu item Raspberry&#8594;Openplotter&#8594;Pypilot. Click it. This brings up your first pypilot User Interface, the openplotter user interface.

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/110822041-f6a76880-8290-11eb-9a82-f24b67e2c4a7.png"/>
<br><i>not good</i></p>

* You will see 3 things that are wrong here. First, it ‘failed to detect an IMU’. I told you: pypilot needs an IMU. The IMU needs to be hooked up with 4 wires to the raspberry GPIO header. If you are smart, you ordered one in advance from the pypilot store, and you just stick it right-way-up onto the gpio header. If you think you were smart and you ordered some clone from some iffy online store, you need to solder wires to connect 3v3, SCL, SDA and GND and keep your fingers crossed.
 
<p align="center"><img src="https://user-images.githubusercontent.com/17980560/110822270-31110580-8291-11eb-8fc6-72dd6e42b019.png"/></p>

* The bottom line on the screen says you need to enable i2c. Go to Raspberry&#8594;Preferences&#8594;Raspberry Pi configuration, click the Interfaces tab, and enable i2c. Go back to the pypilot user interface and click Refresh. It will now see the IMU. 
* Finally, change 'Disabled' to 'Autopilot'. When the changes are applied, you have a running pypilot! Don’t let your tea grow cold.

<p align="center"><img width="80%" src="https://user-images.githubusercontent.com/17980560/112214726-d2983f80-8c1f-11eb-8a0f-6716891fa542.png"/>
<br><i>good</i></p>

***

[Step 3: The openplotter user interface >>>](Step-3-The-openplotter-user-interface)