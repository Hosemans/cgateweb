cgateweb
========

### This fork by MM

This fork has extra functions for C-Bus thermostats.  The HVAC application in C-bus is quite complicated and requires fully formatted messages to be sent.
You can't just send a 'zone on' command for example.  The command needs to include all the parameters, every time.
e.g. C-Bus command "aircon set_zone_hvac_mode //HOME/254/172 5 0 1 0 0 0 1 8 4352 0"
It is essential that you read and understand the C-Gate Air-Conditioning Application User Guide.pdf document from Clipsal.

The problem is formatting the entire C-Bus HVAC command parameters.  This needs to be done via a script from your own system.
Formatting the parameters is beyond the scope of this fork as it depends on how your home automation system is setup.
Once the parameters are formatted correctly they can sent to cgateweb via an MQTT payload.  E.g. "5 0 1 0 0 0 1 8 4352 0"
This fork will then take those parameters and format the entire MQTT topic and paylaod.

This fork will also read incoming C-Bus HVAC messages and format them into MQTT payloads.



MQTT interface for C-Bus written in Node.js

### Install:
(for raspberry pi)
```
cd /usr/local/bin
sudo git clone https://github.com/the1laz/cgateweb.git
cd cgateweb
npm install
sudo nano settings.js
```
Put in your settings.
```
sudo cp cgateweb.service /etc/systemd/system
sudo systemctl start cgateweb
sudo systemctl enable cgateweb
```
### Usage if not using as service:

1) Put your settings in settings.js
2) Run node index.js

Detailed guides and supporting articles will be posted on my blog: https://blog.addictedtopi.com/category/project/cbus/

### Updates get published on these topics:

 - cbus/read/#1/#2/#3/state  -  ON/OFF gets published to these topics if the light is turned on/off

 - cbus/read/#!/#2/#3/level  -  The level of the light gets published to these topics

### Publish to these topics to control the lights:

 - cbus/write/#1/#2/#3/switch  -  Publish ON/OFF to these topics to turn lights on/off

 - cbus/write/#1/#2/#3/ramp  -  Publish a % to ramp to that %. Optionally add a comma then a time (e.g. 50,4s or 100,2m). Also, INCREASE/DECREASE ramp by 5% up or down and ON/OFF turns on/off.

### This requests an update from all lights:

 - cbus/write/#1/#2//getall - current values get published on the cbus/read topics

 #1,#2 and #3 should be replaced by your c-bus network number, application number, and the group number.

Requesting an update on start or periodic updates can be set in the settings file.

### This requests the network tree:

 - cbus/write/#1///gettree - result gets published as JSON on cbus/read/#1///tree

### Other notes:
I edited the original to work with HomeSeer.
There are specific notes in the .js scripts.
It assumes the default cgate ports.
