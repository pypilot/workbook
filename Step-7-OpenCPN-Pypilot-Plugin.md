This is my favourite interface. It is slick and snappy. I am a keen OpenCPN user and use quite a few of the plugins. To install the pypilot plugin:

* Start Raspberry→Openplotter→OpenCPN
* In the toolbar, click the first icon that looks like a gear wheel. For future reference, this is the ‘Options’ menu.
* Click the ‘Plugins’ tab
* In the lower left hit ‘Update Plugin Catalog: Master’. This takes a few seconds; make sure your ethernet cable is in.
* Scroll down to pypilot and click it; click Install, then check Enable and click Ok.
* The toolbar now has a new icon on it that looks like a sailboat. This is the pypilot plugin. Click it.
* Click the second button in the lower area. It says Config but you won’t see that because the window is too small and you cannot enlarge it manually. Click it anyway and select 127.0.0.1 for ‘host’ and click OK.
* At this point, the plugin should connect and become alive. The pypilot button will turn from grey to red, and to green if you engage it. There are a few small differences with the openplotter user interface:
  * If the Arduino is not present, it says ‘no motor controller’, instead of ‘None’. If it is present, it says ‘idle’ or ‘OK’ instead of ‘Arduino’.
  * The gains are now under a button, there is no scope, but there is a statistics screen.
  * Instead of a rudder slider, you now have the familiar left and right buttons:

<p align="center"><img src="https://user-images.githubusercontent.com/17980560/110982344-b5828780-8368-11eb-9178-347daf87390f.png"/></p>
 
<a href="#oldversion">&nbsp;</a>
* At the time of writing, this did not work immediately: the plugin shows 'disconnected', despite pypilot being up and the host address being correct. A quick check reveals the cause: the pypilot software version that is installed by openplotter is old (v0.16), and still listens on the old internal port signalk/21311, instead of the new one pypilotServer/23322; to which port the spanking new OpenCPN plugin is trying to connect to.
* If that is the case, [upgrade pypilot](Step-8-Looking-under-the-hood-of-openplotter#upgrading-pypilot). This is described in the next step.
***
[Step 8: Looking under the hood >>>](Step-8-Looking-under-the-hood-of-openplotter)