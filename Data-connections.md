There are two different types of data connections in pypilot:
* Internal connections
* External connections

## Internal data connections
Before, when we talked about the four User Interfaces, we mentioned this blackboard system, which all pypilot software modules used for internal communication, and that was used by the User Interfaces to interface with the pypilot system. Let’s talk a little bit more about it, and then forget about it.

This blackboard system is actually called pypilotServer and pypilotClient, if you look under the hood. Between the software modules there is some interprocess communication that is used; between User Interfaces and pypilotServer it’s a JSON TCP socket connection on port 23322. The data that is stored in pypilotServer is both very volatile and very persistent: it contains pitch and heading values, that change at 10Hz, but also contains configuration items that are almost fixed. The pypilotServer stores values periodically in a file on the pypilotServer, so that’s how they survive restarts.

For those who have used or studied pypilot before: this mechanism used to be called signalk and it communicated over a different port. This was changed somewhere in 2020, between pypilot 1.x and 2.x, and is why old User Interface clients won’t work anymore with a new pypilotServer, and vice versa.
But to conclude this part: this mechanism is only used to exchange data between pypilot software modules internally, and for User Interface operation. It’s a closed pypilot system and it is not supposed to be used for nautical data.

## External data
External data connections are concerned with nautical data, like heading, wind angles, speed etc. To make it easy to understand, think of nmea data. Pypilot can use, or **consume** data, but it can also **produce** them.
- Nautical data produced by pypilot
  * Magnetic Heading
  * Pitch
  * Heel
  * Other
- Nautical data consumed by pypilot
  * Bearing to waypoint
  * Cross track error
  * Apparent wind angle
  * Speed over ground
  * Apparent wind speed.

At the very minimum, pypilot can do with consuming or producing **no data** at all. It can simply hold the boat on a certain heading using the compass heading from its own IMU. 

The magnetic heading, pitch and heel information that the IMU **produces** might be useful for other consumers on the boat, so it can be made available. 

The nautical data **consumed** by pypilot have already been covered in the preceding text, and it has all to do with the autopilot modes ‘GPS/Track’, ‘Wind’ and ‘True Wind’. If you provide that data to pypilot, those autopilot modes become available, and you can select them in the User Interface.

## External data connections
The external data can be transferred in 2 ways:
* NMEA0183 messages; and 
* SignalK messages.

## NMEA0183 messages
NMEA0183 messages are handled by 
* TCP port 20220; and 
* Serial interfaces.

**TCP port 20220** both **provides** and **consumes** NMEA messages. Just do a telnet to that port, and you will see what that means:

```
pi@openplotter:~ $ telnet localhost 20220
Connected to localhost.
$APXDR,A,1.743,D,PTCH*7A
$APXDR,A,0.277,D,ROLL*6B
$APHDM,21.297,M*0C
```

In the snipplet above you see how you **receive** from pypilot the pitch, roll and magnetic heading. 

You can also **send** NMEA0183 messages to this socket 20220. The following messages are recognized:
* APB (bearing to waypoint and crosstrack error)
* WMV (apparent wind angle and apparent wind speed)
* RMC (speed over ground, lat/lon, and gps timestamp)
* RSA (rudder angle)

It has already been mentioned a few times: for the autopilot mode GPS/Track pypilot needs APB messages, for Apparent Wind pypilot needs WMV messages, and for True Wind pypilot needs WMV and RMC messages.

To interface these nmea messages to and from pypilot, you can configure your multiplexer to do so. There are various options: you can use a hardware multiplexer, OpenCPN Data Connections, kplex, signalk.

One very special option is the **Forward NMEA** checkbox in the OpenCPN pypilot plugin. If you check that box in the plugin, all these NMEA sentences are automatically synchronized between OpenCPN and pypilot. No questions asked. Quite neat, now you know the background, right?

**Serial interfaces** also consume NMEA messages. For instance, you can plug a NMEA0183-to-USB port into your machine with a wind vane attached to it, the linux kernel will register this as a serial interface, and pypilot will scan it, probe it with 4800 and 38400 baud, and use its information. This also applies to the native UART interface if that is free. As for **providing** NMEA messages over serial interfaces; this seems not to be done. 

## SignalK messages
Until recently, making these external data connections with NMEA messages over TCP port 20220 was the only option. Recently, a more modern option has become available, and this is signalk. I’ll explain more about signalk later, but if you have it set up properly, all communication of nautical data is automatically taken care of. From the pypilot perspective, this is zero configuration. From the signalk’s perspective, you must make sure mdns is switched on in the Server settings, and you must approve the Access Request in the signalk Security menu.


***
[The Steps >>>](The-Steps)
