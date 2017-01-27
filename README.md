# Commandfusion-MQTT-Plugin
MQTT Plug-in for Commandfusion iViewer
Simple MQTT Client for Commandfusion iViewer

Usage is quite simple:

Setup:
1. Add MQTT.js file to Project Script Manager
2. Create TCP System with link and port of your MQTT Broker. (Standard MQTT port is 1883)
3. Create first two commands - MQTT_Connect and MQTT_Subscribe. Those should only have appropriate javascript code(example in script)
4. Create a macro - MQTT_Start, which should call two commands above with 100ms Interval(first connect, then subscribe). Assign this macro to Startup macro in System Properties.
5. Create single feedback with (.*) regex.

Use Add Command to generate publish messages. Just put MQTT_Publish function in Javascript field,according to example in script.
For example:

For switch:
Field Javascript: MQTT_Publish("/myhome/command/Light_FF_Bath_Ceiling/state","ON", system)

For slider:
Field Command Value:{{[sliderval]}}
Field Javascript: MQTT_Publish("/myhome/command/Temperature_Setpoint/state",data, system)
Do not forget to set "Javascript sends command value" flag

To process feedbacks:
1. Create a UDP Socket Loopback system with IP 127.0.0.1
2. As End Of Message Character define \x0D in System Properties of this system
3. For each feedback create new feedback and in regex provide appropriate regex assuming that your incoming data in this case will have a look like topic=value.For example:
/myhome/in/Light_FF_Bath_Ceiling=(OFF)
/myhouse/office/Room_Temp=(.*)

Some improvements are planned:
- packet length is calculated a bit primitive way, and not according to standard
- Security/Authorization to be added
