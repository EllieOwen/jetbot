# jetbot
This is a robot that will try to be similar to the NVIDIA Jetbot.  Instead of 3D printing a chassis designed by the NVIDIA Jetbot team we will start with a 2-wheel self balancing robot.  The NVIDIA Jetson Nano will be used and also will have vision.

The brains of the robot will be the NVIDIA Jetson Nano.  It's on backorder and I hope to have it before Memorial Weekend. 
https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit

Here is some information on the NVIDIA Jetbot:  https://github.com/NVIDIA-AI-IOT/jetbot

This is the self-balancing robot we're starting with:  https://www.banggood.com/DIY-Smart-RC-Robot-Car-Self-balancing-Car-APP-Control-Compitable-With-Arduino-p-1427016.html?utm_design=41&utm_source=emarsys&utm_medium=Shipoutinform171129&utm_campaign=trigger-emarsys&utm_content=Winna&sc_src=email_2671705&sc_eh=baaf08149f00f9ea1&sc_llid=12393951&sc_lid=104858042&sc_uid=ALdPQMSVJ1&cur_warehouse=CN
It's being shipped right now and I'll update this post when I receive it and see what's in the kit.  Estimated delivery date is May 24th.

I also have a lot of parts that could be used to make a robot from scratch.  I have motors, motor controllers, Arduinos, Raspberry Pies, 9 DOF IMU, lipo batteries, etc. If anyone want specific information on these parts let me know.

I'll try to work on the Wiki to help document the project.

First steps (jud)
Heres the general project outline from Jud's point of view:
Step one is to get the project all organized on Github. To start we need to merge two existing projects: Jetbot and self-balancing car. The jetbot project is already on gethub so we just need to clone it. The balancing car (from banggood.com, above so we call it a Bang Car) has some offbrand Arduino clone which has some software that drives the Bang Car somehow which there's no documentation so we'll figure that out when we get it and add whatever software to the Gethub project. If the balancing Bang Car works and we have source code then the quickest way to get the jetbot project running on the Bang Car chassic is to make it's onboard controller emulate the Adafruit product 2927 motor control board used by the jetbot project which talks I2C (https://www.adafruit.com/product/2927). Also, we need to power the Jetson Nano from the 11.1 Volt, 1.9 amp hour battery that's incorporated in the Bang Car. The best way to do this is to make DC to DC that makes 5 volts, 4 amps for the Jetson Nano from the 11.1 rail on the Bang Car. It should also charge the Bang Car 11.1 volt battery from the Jetson 5V rail so that plug a 5 Volt 5 or greater amp supply into the Jetson Nano barrel jack to run and charge the whole thing when its not moving. One of the first things we want to make it do is plug itself in and charge. 
The bidirectional 30 watt DC to DC will be made with parts I have on hand and incororate whatever processor is on the Bang Car and talk to the Jetson Nano through the same I2C channel used to control the motors.
I think an early step should be to replace the motor driver board on the Bang Car (which is LM298 based and limited to 2 amps) with a board that has FETs and handles at least 10 amps per motor like https://www.amazon.com/Controller-DROK-H-Bridge-Brushed-Regulator/dp/B078TFLD7Q?ref_=fsclp_pl_dp_1
Also, we'll want to replace the off-brand arduino on the Bang Car with a Teensy 3.5ish or I have a Maple arduino form factor board with an STM Arm 4 based MCU on it which almost as good as a teensy. I'm interested in doing the Bang Car control software and I don't want to try to code it on something that doesn't have way more than enough power to do it right.