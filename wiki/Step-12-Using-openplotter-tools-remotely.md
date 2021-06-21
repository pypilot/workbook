As said before, the tools on openplotter default to connect to the pypilot on _localhost_. They can, however, be used on a _remote_ pypilot. This is useful when you want to connect to an attached tinypilot.

This applies to the following scripts:
* pypilot_client [-s ip-address]
* pypilot_client_wx [ip-address]
* pypilot_calibration [ip-address]
* pypilot_control [ip-address]
* pypilot_scope [ip-address]

After a successful connection to a remote pypilot, the IP address is stored in the file ~/.pypilot/pypilot_client.conf. Any subsequent invocation of any of the scripts above will default to that IP address. 

For remote access, pypilot_client needs to be invoked with -s <IP-address>. For the other scripts, the -s switch is not needed.

Example:

```
# In this example, 10.10.10.2 is the address on the tinypilot
pi@openplotter:~ $ pypilot_client -s 10.10.10.2
[whole database dumped]
pi@openplotter:~ $ pypilot_client -s 10.10.10.2 -c ap.heading_error
ap.heading_error = -2.122
ap.heading_error = -2.154
ap.heading_error = -2.158
^C
pi@openplotter:~ $ pypilot_calibration
[...]
```
This does not only apply for when you invoke these scripts from the command line, but also when you invoke them from the openplotter interface: from then on, your pypilot is the external one. If you ask me, this is a good example of how well-implemented a modular architecture can be.
***
[Step 13: SignalK connections >>>](Step-13-SignalK-connections)

