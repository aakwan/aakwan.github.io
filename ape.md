---
title: Assitive Pulley Exoskeleton (APE)
---

## Introduction
The purpose of this robot is to apply 15 N*m of torque at the hips in order
to assist able-bodied people with walking and eventually squatting. An assistive force is created
through an oscillating pulley that is attached to a DC motor and pulls at the end of every half gait
cycle to help with the ankle push off of walking. Bungee cords are attached to behind the knee in
order to apply this force as well as apply an upward force when a person squat. The housing of the electronics, pulley, and
guides for the bungee were 3D printed. The DC motor is a windshield wiper motor and the
mounting plates were laser cut out of wood. An incremental rotary encoder and an MPU 6050
are used to translate the roll motion of the leg into rotational motion of the pulley. The suit was able to successfully follow the gait cycle of a person as they walked. The steps that were not achieved was applying the assistive force at the hip in a reliable way. This was due to the limitation of the microprocessor and future work would have included integrating a Raspberry Pi to have more memory so that the system could utilize the IMU sensor data to dictate the phase of the gait cycle. 

## Pulley Versions
The following pictures are the design iterations for the pulley attached to the DC motor. The 
models were generated using Solidworks.

**Version 1**
<img width="100%" height="100%" align="center" src="https://aakwan.github.io/Images/Pulley V1.png">

**Version 2**
<img width="100%" height="100%" align="center" src="https://aakwan.github.io/Images/Pulley V2.png">

**Version 3**
<img width="100%" height="100%" align="center" src="https://aakwan.github.io/Images/Pulley V3.png">

**Version 4**

Front of the Pulley
<img width="100%" height="100%" align="center" src="https://aakwan.github.io/Images/Pulley V4 Front.png">
Back of Pulley
<img width="100%" height="100%" align="center" src="https://aakwan.github.io/Images/Pulley V4 Back.png">

**Version 5**
<img width="100%" height="100%" align="center" src="https://aakwan.github.io/Images/Pulley V5.png">

## Videos

Video of testing intertial mesurment unit (IMU) integration with PID loop control of DC motor.
<video src="https://aakwan.github.io/Videos/Test.MOV" controls="controls" width="100%" height="100%">
</video>

Video of passive walking test with integration of IMU sensor loop and tuned PID loop.
<video src="https://youtu.be/tk-KNEG1uyw" controls="controls" width="100%" height="100%">
</video>

<p align="center>
<iframe width="560" height="315" src="http://www.youtube.com/embed/tk-KNEG1uyw" title="Yotube Video Player" frameborder="10" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p>
                                                                                                                        

[Return](/index)
