---
layout: post
title:  "SensorTag and Node-Red"
date:   2017-08-07 11:10:00 +0000
categories: node-red
---
This experiment uses both a SensorTag and a Raspberry Pi

## Bluetooth LE on Raspberry Pi

My Pi does not have BLE support as standard, so a BLE dongle is used. The basic instructions are here
https://developer.ibm.com/recipes/tutorials/ti-sensor-tag-and-raspberry-pi/

Some commands:
```
sudo hciconfig [ up | down | reset ]
sudo hcitool lescan
```

## Pi Node-Red flow

The purpose of this flow running on the Pi is to connect to the sensortag using the sensortag node
installed by ```npm i node-red-node-sensortag```.

```
sudo node-red-start & 
sudo node-red-stop &
```

The flow supports some URL endpoints to set a global variable that controls the flow of the data to Watson IoT service.

```
bjraspi.local:1880/[ start | stop | status ]
```