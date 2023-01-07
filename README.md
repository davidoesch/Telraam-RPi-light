# Telraam-RPi-light
## Overview
This a python only Version of [Telraam](https://github.com/Telraam/Telraam-RPi/). Telraam is an affordable and yet intricate traffic-monitoring network built around a cluster of low-cost sensors and a central server. It is a collaboration between Transport &amp; Mobility Leuven (TML), Mobiel 21, and Waanz.in, that is financed by the Smart Mobility Belgium Grant of the Federal Government. Mine is observing the [Gaselstrasse in Schliern ](https://telraam.net/home/location/9000004609)

## Why a python only Version Telraam for the Raspberry Pi 
Had  an old  Raspberry pi Model B+ ( yes really old ...) with a camera modul which does a marvelluos job as weather station and some houskeeping stuff. But I did not wnat to use the [official Telraam raspian image] (https://github.com/Telraam/Telraam-RPi#telraam-as-an-sd-card-image)  since I want to run more on the pi than only observing the road. ANd forthermore , no WLAN on my Pi. SO I w just want run telraam as a deamon

## Installation steps
I followoed along the line of https://github.com/Telraam/Telraam-RPi#telraams-internal-nuts-and-bolts 

- Register your Device 
use the process defiend by Telraam [https://telraam.net/home/self-measure]. 
To get your TelraamID ( which is the MAC address)

in python on your pi run
```
import uuid
mac_addr = uuid.getnode() 
print(mac_addr)
```
That is your telraam ID


- Install dependencies 
```
sudo apt-get install openexr #needed for some camerea SW
python3 -m pip install --no-cache --force-reinstall --no-deps opencv-python==4.5.3.56 --index-url https://pypi.org/simple #CV2 without wheels
```

- The Code
copy https://github.com/Telraam/Telraam-RPi/tree/master/Image%20processing directory to your home/pi location into /home/pi/Telraam
All the python code needed is in telraam_monitoring.py


- Start the deamon on boot (Systemd)
```
sudo nano /etc/systemd/system/telraam.service
```
add
```
[Unit]
Description=Telraam monitoring service
After=multi-user.target

[Service]
Type=idle
User=pi
Group=pi
WorkingDirectory=/home/pi/Telraam
ExecStart=/usr/bin/python /home/pi/Telraam/telraam_monitoring.py --rotate 0 --idandtrack

[Install]
WantedBy=multi-user.target
```
then

```
sudo systemctl enable telraam
systemctl start telraam
```
Check if everything runs
```
systemctl status telraam
```

Then just login in your Telraam account in the dashboard

Done

