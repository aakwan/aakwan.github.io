---
title: The Efficiency of Spine Stiffness for Anguilliform Locomotion
---



_**Abstract**_ **- In this paper, we discuss the methods and requirements to simulate a soft bodied beam using traditional rigid body kinematics to produce motion inspired by eels. Eels produce a form of undulatory locomotion called anguilliform locomotion that propagates waves throughout the entire body. The system that we are analyzing is a flexible 3D printed beam being actively driven by a servo motor. Using the simulation, we also analyze different parameters for these spines to maximize the linear speed of the system.**

## 1. Introduction

The locomotion of fish has been a well studied topic over the past few decades. Due to the developments of high speed video cameras, researchers have been able to gain a better understanding of fish locomotion, especially body/caudal fin pulpursion (BCF). BCF is the classification of swimming that generates propulsion through propagating waves through their bodies [1][2]. Eels utilize anguilliform locomotion, which exhibits the largest propagation of waves through the body relative to other forms of undulatory propulsion as seen in Fig. 1 (a). We are focusing on the motion of eels as opposed to other undulatory movements because they utilize the majority of their body to generate propulsion. Additionally, their body anatomy is very simplistic, largely consisting of the singular body fin as seen in Fig. 1(b). This makes the inspired design significantly less complex as no additional pectoral fins are necessary to generate this locomotion.

![](RackMultipart20220322-4-18z5m5t_html_7f0dafd1b547dd8.png)

**Fig. 1.** (a)Four classical undulatory propulsion with corresponding gait cycles [1]. This figure highlights the phases and displacement of the fish during anguilliform locomotion. (b) Side profile of eels, noting that the body is the major fin of anguilliform propulsion [1].

  A. _Eel Inspired Robots_

Previous research on bio-inspired eel robots has been developed as soft bodied devices and classical rigid robots. Nguyen _et al._ [3] developed an inflatable balloon actuator that requires being connected in parallel and in series in order to produce this sinusoidal propagation. Fremerey [4] utilized a singular inflatable actuator to generate this motion. Both researchers displayed a good method of producing undulatory motion and variability of propagating a wave. However, the method to manufacture the actuator is highly complex and requires a multitude of steps and tools to produce. On the other hand, Fremerey [5] and Chen [6] utilize a tunable, spring rotational joint that is connected in series to produce the propagation. This device displays the capabilities of rigid body systems, but requires a highly complex system and dozens of parts in order to generate this motion. Regardless of either method, both soft and rigid devices require highly complex and multiple actuators in order to create propulsion.

  B. _Motivation_

From the previous research done with regards to eel inspired robots, this research is inspired to investigate a system that requires only a single actuator with a flexible spine and determine a method in which to model the system using classical rigid techniques. The goal of this research is twofold:

1. To build a rigid body model in simulation to describe the soft beam system
2. Investigate the design parameters of the spine and tune it to maximize the speed and wave propagation

## 2. Approach &amp; Methodology
  A. _Prototypes_

In order to better understand the relationship of the material characteristics to damping and spring constant values, multiple spines with varying tapers along the length of the beam were printed as seen in Fig. 2. The spines were printed as solid Thermoplastic polyurethane (TPU) beams. The length of each spine was 0.25m as shown in Tab. Ⅰ because this was the limitation of the print bed. Longer spines could have been created through joining multiple segments but in order to minimize exterior factors, it was best to use only one segment to isolate the characteristics of the spine.

![](RackMultipart20220322-4-18z5m5t_html_86e47a47c905497a.png) **Fig. 2.** The 3D printed spines are made from TPU. From top to bottom the tapers of each spine are 0, 2.25, and 4.5 degrees.

By creating different tapers, the objective was to investigate and isolate the characteristics of damping and stiffness with regards to TPU. The taper along the length was determined to generate the stiffness gradient because it was the only direction of taper that could be consistent with the method of manufacturing. A taper along the width of the spine could not be repeatable due to the variation in layer heights with different tapers when 3D printing. This would result in a varied amount of material depending on the taper and not produce meaningful results because of this irregularity.

  B. _Kinematic Model_

To better understand the characteristics of the flexible spine, we chose to estimate the system as a series of flat plate links and torsional springs as seen in Fig. 3. The spine was broken into equal segments where the damping and spring constants were scaled using the equations below:

$b_seg$ = $b$ * $A_cs$ * $m_seg$ (1)

(2)

![](RackMultipart20220322-4-18z5m5t_html_9c009a92c5989f61.png)

**Fig. 3.** Multi segmented model of a spine with _n_ segments, where b1…bn = scaled damping coefficient (eq. 1), k1…kn = scaled spring coefficient (eq. 2), cm1…cmn = center of mass of segment, θ1…θn = angle of rotation relative to global frame, Fd\_1…Fd\_n = drag force of surrounding water on each segment, FR is the drag force of the front cross section.

Where bseg and kseg are the scaled constants, Acs is the cross section of the spine across the Y-Z plane, and mseg is the mass of the spine segment. The damping and spring coefficient relative to the taper, b and k respectively, were calculated using experimental data which will be discussed later in the paper. These scaling factors were selected because they are the major differences between each segment. In both eq. 1 and 2, the amount of material at each joint affects the output force generated. In linear damping, the mass is a factor in calculating the damping ratio so it was assumed that the mass of each segment would have an effect in this model.

From Fig. 3, the aerodynamic forces are being applied orthogonally to each segment and to the head of the spine. The head is the portion of the spine that is attached to the mounting hardware to suspend the system in water as seen in Fig. 5. The equation used to calculate the drag force is as follows:

(3)

Where _C__d_ is the drag coefficient, _⍴_ is the density of water, _v_ is the velocity, and _A_ is area of the segment. For this model there are two drag coefficients, the drag coefficient of a flat plate is 1 and the frontal drag coefficient is dependent on the testing configuration and was determined through experimental set up and will be discussed further on in Section Ⅱ.D.

![](RackMultipart20220322-4-18z5m5t_html_3539dca06286b216.png) **Fig. 4.** Testing configuration to measure dynamic tests. Reflective marker at the non fixed end tracked through an Optitrack system. Left is the neutral state and the right is drawn 100mm to the right.

  C. _Experimental Setup_

As mentioned previously, the damping, spring, and drag coefficients were highly dependent on the test configuration and design used for the spines. To get a better understanding of these properties we conducted a dynamic, frontal drag, and constrained swimming experiment

_Dynamics_

The spines were attached to a fixed plate and a reflective marker was attached to the free end of the spine as seen in Fig. 4. The spine was then pulled back to 100mm by hand with a string and then released. The tracked data was then recorded using an Optitrack track system that measured at 360 fps.

_Frontal Drag_

To calculate the frontal drag coefficient and effective surface area, we attached a force sensor to the spine and dragged it through stable water conditions of a tank seen in Fig. 5.

![](RackMultipart20220322-4-18z5m5t_html_fd9a638f06578371.jpg) ![](RackMultipart20220322-4-18z5m5t_html_5c97c8ea7a56a49d.jpg)

**Fig. 5.** Carriage testing configuration of spine and force sensor for both servo and robotic arm tests, left and right respectively.

Due to the frontal surface area being very small it was assumed that the results would be very small. Knowing this, two tests were conducted experimentally to verify the constant. The first method was using a servo and string to pull the carriage at a constant velocity and measure the force against the spine. This can be seen in Fig. 6 (a) and used a string to pull the spine while another string was used to stabilize the carriage while the servo accelerated to the constant velocity. The second method was using a robotic arm to drag the carriage in the water as seen in Fig. 6 (b). The purposes of this second experiment is that the robotic arm has the capabilities of higher precision constant velocity and it eliminates the need of a secondary apparatus to stabilize the carriage, thus eliminating additional external variables into the system.

![image9.jpg](RackMultipart20220322-4-18z5m5t_html_105509b995fdaf12.gif) ![image10.jpg](RackMultipart20220322-4-18z5m5t_html_ef4e9b2cb7f5d412.gif)

**Fig. 6.** Testing set up in a water tank. (Top) Blue line is a string pulled by a servo and the orange line is a string to stabilize the carriage. (Bottom) UR-5 robot initial configuration to drag the spine through water.

_Constrained Swimming_

For the constrained swimming experiment, the spines were mounted to a similar carriage as in Fig. 5 but instead of a force sensor, a servo was attached to the spine and was put in a test set up like Fig. 6 (a) to allow for free planar motion in the tank. The purpose of this test was to analyze how the spines would work under swimming conditions. The data would then be compared to a swimming simulation of the spines to determine how well the model matched the physical system. This will be further discussed in Section Ⅳ.

  D. _Data Collection_

_Dynamics_

From the dynamics test, the data collected for each taper is shown below in Fig. 7. The data was cut off at 80% of the 100mm pull back position of the test denoted in the cut data of Fig. 7. The reason for this was to ![](RackMultipart20220322-4-18z5m5t_html_875b1ccdf3668f33.png) **Fig. 7.** Data collected from dynamic spine test. The full data is the entire test and the cut data is truncating data after the tip reaches 20% of the initial state.

avoid the inherent viscoelastic hysteresis of the material. This property of TPU and other flexible materials is what causes the material to come to rest close to the neutral position and then creep over an extended period of time to reach the final neutral position. This characteristic is exhibited from 3-6 seconds in Fig. 7 as the spines slowly return to the zero position. If included, this data would therefore artificially decrease the damping and spring constant due to implying that the spines are stiffer than in reality.

_Frontal Drag_

In the frontal drag experiment, the velocity and drag force were the design parameters and measured results for this test. Using eq. 3, the following front drag constant was derived:

(4)

The effective drag area was unknown for the system, which is why it is accounted for in the frontal drag constant. In collecting the data it was observed that the measured force was in the range of 0.04-0.01 N which was within the noise range of the sensor which can be seen in Appendix Ⅰ. Therefore, it was difficult to calculate a constant that accurately reflected the system. Based on the data collected from the experiment an estimated value was shown in Tab. 1. The results of the frontal drag experiment did not lead to any direct results and a higher resolution sensor is necessary to determine conclusive results.

_Constrained Swimming_

The results of the swimming experiment are shown in Fig. 8 below. This data will be ![](RackMultipart20220322-4-18z5m5t_html_d23b5f487839ec48.png) **Fig. 8.** Data collected from the constrained swimming tests. The data is only plotting the linear motion of the head along the direction of motion.

compared with the simulated swimming experiment later in the paper in Section Ⅳ. The servo used for the experiment was controlled via position and had a sweeping angle of 44 degrees at a frequency of 1 Hz.

This sweep angle and frequency were held constant for all three tests. High speed videos were then recorded of each spine and used for data collection. This data was collected through using a tracking software called Tracker to plot the position of the head in recorded videos of the experiment.

## 3. Simulation Model

Next, I used the python package pynamics to solve for the equations of motion to simulate the system designed as seen in Fig. 2 and 9. In order to accurately represent the proposed system, it was determined to represent the tapers of the spine as a series of stepped flat plates seen in Fig. 9. The reason for this was because the inertia equations could be easily

_Table Ⅰ_

Simulation Constant Variables

| **Name** | **Value** | **Units** |
| --- | --- | --- |
| Length | 0.25 | m |
| Width | 0.04 | m |
| Height | 0.003 | m |
| TPU density | 1220 | kg/m^3 |
| Water density | 998 | kg/m^3 |
| Front Drag | 0.001599 | m^2 |
| Frequency | 1 | Hz |
| Amplitude | 44 | deg |

simplified through a flat plate model and the aerodynamic forces could use coefficients related to flat plates. Additional constants and variables were needed for the simulation and can be seen in Tab. Ⅰ.

Using the simulation, the next step was to fit the data from the dynamic test to solve for the

![](RackMultipart20220322-4-18z5m5t_html_672ab102d0656447.png) **Fig. 9.** (a) Frame diagram of rigid body representation as a three link system. Representation of tapers in a three(b) and five(c) link model where the green dashed line is the actual spine with constant stiffness gradient and black lines are the mass and inertial representation in the model.

damping and spring coefficients discussed in Section Ⅲ. In order to do this, it was first necessary to solve for the initial conditions of each joint. The reason for this was to match the initial position of the tip in each experiment and also to minimize any ripples created from not correctly setting the initial joint states. These values were calculated through applying a constant force perpendicular to the tip of the spine. Then using the SciPy python package we optimized the force magnitude to match the initial displacement of the spine and recorded the initial rotations of each joint. These were then used as the initial conditions of the simulated dynamic test.

An evolutionary algorithm, CMA-ES was used to fit the simulated to experimental tip data to solve for the damping and spring constants as seen in Fig. 10. The results for each taper were then recorded and plotted and linear regression lines were calculated. To determine the spine model that best fit, a

![](RackMultipart20220322-4-18z5m5t_html_885d413b3bec5549.png) **Fig. 10.** Results of fitted damping and spring constants using CMA-ES for a 5-link model.

variable number of segments were used to determine the best results. The five link model had the highest R2 values and therefore was considered to have the best results. The resulting fit model is shown in Fig. 10 and the lines of regression were as follows:

(5)

(6)

Where b and k are the damping and spring force respectively and x is the taper in degrees. The CMA-ES results follow the trend but do not match the data. This is likely due to the data not being zeroed and the viscoelastic property. Equations 5 and 6 were then integrated into the python model. This now allowed any taper value to be inputted into the simulation and resulted in damping and spring constants for that designated taper.

Global x and y positions were added to the simulation to allow for the simulated spine to move in free space. Additionally, dynamic constraints were added to the system to model the position control of the motor seen in eq. 7 below.

(7)

Where _A_ is the amplitude, _f_ is the frequency, and _t_ is the simulation time. Both the amplitude and frequency values are defined in Tab. Ⅰ. The dynamic constraint is then applied to the first link of the simulation, to mimic the torque applied to the head of the swimming experiment.

## 4. Results and Discussion

The results of the python model compared to the constrained swimming test experiments denote that the model produces similar trends and velocities as the swimming test. This can be seen in Tab. Ⅱ. For the 0 and 2.25 deg tapers, there are larger differences between the two velocities due to two main discrepancies between the model and experiment.

_Table Ⅱ_

Spine Head Velocities

| **Tapers (deg)** | **Simulated (m/s)** | **Experimental (m/s)** |
| --- | --- | --- |
| 0 | 0.188456 | 0.151990 |
| 2.25 | 0.171553 | 0.114535 |
| 4.5 | 0.085856 | 0.086621 |

The first is that the model does not take into account the turbulence created from anguilliform locomotion. The sinusoidal propagation down the spine creates vortices in the water which traditionally have a positive effect on the thrust created by the eel [1]. However, since the swimming experiment was conducted in a tank, the vortices and turbulence created by the device rebounded against the walls of the tank, thus impeding the motion of the spines. These forces can not be calculated in the python model generated and would require additional software to calculate.

The second discrepancy is that the frontal drag constant plays a significant role in tuning the model to the experimental data. Since the constant is an estimate based off the experiment discussed previously, future work would include further analysis and tuning of this value. These errors are validated in steep taper data of Tab. Ⅱ. The steep taper spine (Fig. 2) has the least surface area which results in the smallest aerodynamic forces applied onto the spine. This suggests that the rebound turbulence of the tank has the least effect of the three spines and that the drag constant is still too large in the python model.

## 5. Conclusion and Future Work

Despite these two errors between the model and experiment, the velocities and motion correlated very well. While the velocities did not directly match, both the simulation and experiment exhibited the trend of decreased velocity as the taper increased. Therefore, using a rigid body approximation of the soft system is sufficient. Additionally, external forces of fluid dynamics and precise tuning of coefficients are not necessary to model the system. Additionally, it is not necessary to have complex soft or rigid body systems to create bioinspired anguilliform locomotion. Through a simple actuated soft spine, it is possible to accurately simulate and design an inexpensive and simplified robot.

As mentioned previously improvements could be made to the drag constant and further investigation could be made into extending the number of links in the model to more accurately represent the spine. Additionally, more research could be done into different design parameters of the spine such as changing the width of the taper, length of the spine, and adding point masses along the spine to increase wave propagation. The secondary objective was to change the design parameters of the spine and deduce spines that had increased efficiency. Various simulations need to be run of the spine to deduce parameters that have a greater effect on the linear velocity of the device. Unfortunately, due to the time constraints, it was not possible to achieve this fully and still requires additional work to design an algorithm to organize these parameters.

A secondary swimming test where the device could be in a larger open body of water would be a better test of the overall device. The reason for this is that the turbulence of the device spine would have a lesser effect on the performance of the spine since no rebound forces would be generated. Also, by not constraining the planar movement and having a short linear distance, it would be possible to better compare the simulation to the physical device. This is because as seen in Fig. 8 the spines decelerated as they approached the end of the tank at 0.5m. If there was a larger run distance, the device would be able to achieve a more accurate velocity.

## References

[1] G. V. Lauder and E. D. Tytell, &quot;Hydrodynamics of undulatory propulsion,&quot; _Fish Physiology_, pp. 425–468, 2005.

[2] E. D. Tytell and G. V. Lauder, &quot;The hydrodynamics of Eel Swimming,&quot; _Journal of Experimental Biology_, vol. 207, no. 11, pp. 1825–1841, 2004.

[3] D. Q. Nguyen and V. A. Ho, &quot;Kinematic evaluation of a series of soft actuators in designing an eel-inspired robot,&quot; _2020 IEEE/SICE International Symposium on System Integration (SII)_, pp. 1288–1293, Jan. 2020.

[4] M. Fremerey, L. Fischheiter, J. Mämpel, and H. Witte, &quot;Locomotion Study of a single actuated, Modular Swimming Robot,&quot; _Design and Nature V_, 2010.

[5] M. Fremerey, L. Fischheiter, J. Mämpel, and H. Witte, &quot;Locomotion Study of a single actuated, Modular Swimming Robot,&quot; _WIT Transactions on Ecology and the Environment_, 2010.

[6] Chen, Z. Wu, H. Dong, M. Tan, and J. Yu, &quot;Exploration of swimming performance for a biomimetic multi-joint robotic fish with a compliant passive joint,&quot; _Bioinspiration &amp; Biomimetics_, vol. 16, no. 2, p. 026007, 2020.

## Appendix Ⅰ

Results of Frontal drag experimental test.

| **Test** | **Velocity (m/s)** | **Force (N)** | **Drag Constant ()** |
| --- | --- | --- | --- |
| Servo | 0.048 | 0.0420 | 0.036564 |
| Servo | 0.072 | 0.0388 | 0.015030 |
| Servo | 0.096 | 0.0372 | 0.008112 |
| Robot | 0.10 | 0.0115 | 0.002296 |
| Robot | 0.15 | 0.0214 | 0.001904 |
| Robot | 0.20 | 0.0271 | 0.001358 |

[Return](/index)
