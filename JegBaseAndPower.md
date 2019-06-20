Motor Calculations:
Starting with the banggood.com Balancing Car kit which includes two motors with 30 to 1 gear reductions and a 1.9 Ah, 11.1 Volt battery,  2.5? inch diameter wheels, a chassic and circuit boards.
No load wheel axel speed is 330 rpm at 11.1 volts, 350 rpm at 12 volts.
That's 9,900 rpm for 11.1 volts at the motor which is 892 KVM (rpm per volt)
Amps / footpound = KVM / 7.0432 so that's 127 amps per foot-pound at the motor and 4.22 Amps per footpound at the wheel.
The maximum torque is determined by the friction of the tires against the road. If the coefficient of friction is 0.80 and the total weight is 20 ounces (1.25 pounds) and the wheel radius is 1.25 inches for example. Worst case, all the weight could be on one wheel. So 1.25 pounds * 0.8 pounds per pound * 1.25 inches / 12 inches per foot = 0.104 foot-pounds torque on the axle is the most you should see.
When we add an active suspension, we'll be making five or ten times the effective weight and need five times the torque but, for now lets call it 0.2 foot-pounds per motor because it works out later.
The speed should be at least ten miles an hour, maybe 20. Ten mph is 1345 wheel RPM.
So 1345 RPM * 30 RPM per RPM / 11.0 Volts = 3667 KVM at the motor (17.4 amps per foot-pound at the wheel).
The 3667 KVM rating is typical of 1/10th scale RC race car motors and Leon has a matched pair in that range.
Such motors would need 3.48 amps to make the required 0.2 foot-pounds of torque at the wheel.
We could get 0.46 foot-pounds from the 8 amp half-bridges on the motor control board I found.
 https://www.amazon.com/Controller-Yeeco-H-Bridge-controller-Regulator/dp/B07D9D5X55/ref=sr_1_32?keywords=motor+controller+board&link_code=qs&qid=1557287553&s=gateway&sourceid=Mozilla-search&sr=8-32 

Motors and Power
I'm going to use two dual brushed motor controller boards (four, 8 amp, half-bridges each, $18 each), a teensy 3.5, the instrument amps I've got (to measure volts and current) and a battery balancing card.
One motor board I'm going to use to make four dc to dc converters: One for each motor to make a seperate 4 to 11 volts at 8 amps, from the battery, to connect to the other motor board and power each motor. 
One dc to dc makes 5 Volts at 5 amps to power the Jetson Nano, the cameras and PiZero (for other camera), the WiFi, a router, etc
The last dc to dc is used to charge the battery at up to 4 amps from 12.6 to 48 Volts so we can charge on-board from a car or any car battery charger.
When we go to brushless motors I just need to add another of the same motor controller boards, more instrument amps and maybe, another teensy.
To start, the Teensy will look like the Adafruit motor controller (https://www.adafruit.com/product/2927) that the Jetson (JetBot project) expects and translate this input to the required signals for the fake Arduino that comes with the Banggood Balancing Car kit while operating all the power supplies.
By monitoring the fake Arduino, the Teensy will learn to take over.
Seven current and seven voltage measurments are required: one for each motor, each dc to dc and the battery. I have AD8214 and shunts for the current measurement and op amps and presicion resistors for the voltage. All will connect to the A/D channels on the Teensy and fake Arduino. When we go to brushless, I want to use current sense transformers and delta sigma converters.
I'll need to fabricate one board with the op amps on it and another with the capacitor and inductors for the dc to dcs. I have plenty of caps and inductors in my junk.

Need to buy:
2 motor control boards $18 each
1 3 cel balancer $14
1 teensy 3.5 $25
10 SOIC to thru-hole adaptors $10

Some changes:
Leon wants to make another "bottom end" from old drill motors and I'm thinking, a more unversal setup to minimize effort across a line of various two wheeled vehicles is the way to go. Also, we'll eventually want to add active suspension with linear motors, brushless motors (for the wheels) and various payloads with various electrical demands. For all motors (muscles), we need to measure the intregral of the current and voltage not just descrete samples and we'll want to use current sensing transformers and hall effect sensors. In any case we need integrators or delta sigma convertors. I want to standardize the processor and analog sensing for all our projects. The Teensy 3.2 thru 3.6 is the best choice for the MCU so we just need to design a standard analog board.

I've been working on a combination analog to digital converter that has a delta sigma front end that feeds the Successive Approximation A/Ds on the MCU (Teensy). I guess you could call it Analog (it's a little different than multi-bit depth) Decimation but I'm not the guy you want to name things (I can't remember or spell proper nouns) The design is pretty complicated and requires a lot of parts so I'm still looking for a chip that will do the job. In the meantime the analog board design (below) at least illustrates the requirements of a brain style control hardware. Brain Style means it drives the motors like a brain would if measured at the motor. The Teensy has enough power to be the immediate brain piece we don't need a full blown neural network. We can hardcode a lot of it since we know the set of parameters that needs to be learned. 

The brain piece is a machine learned model simulation of the Jeg (a Jeg is a two wheeled balancing vehicle with active suspension). The model is the physics data: dimensions, masses, centers of mass, moments, torques and forces such that if you know all the torques and forces you can figure out what the accelerations and their double intergrals are going to be. You know the torques and forces from the currents and voltages. You know those from preplaned maneuvers based on machine learned elemential maneuvers and high resolution measurements. The simulation is kept in sync with reality from current, voltage, IMU, and camera feedback. But you want to be able to go quite a while with just current and voltage and a really long time with IMU. Each motor and sensor has its own simulation nested one layer deep in the simulation of the whole Jeg.

To be high performance, we need a high fidelity model of the motor which includes the inductance as a function of position and current, position as a function of the integral of the back emf, torque as a function of current and position, etc. Anyway, conventional, descret, sampling (successive approximation) does not cut it (no matter how fast or deep), You need an analog integral of the motor current and voltage. That's six integrals for a brusless motor or a 3 phase linear motor.

A delta sigmal converter would work but not exactly the way I'd like it to:
An integrator is made from a capacitor and an opamp. You need to use the right kind of capacitor that is stable and constant over temperature etc and a modern cmos, low offset, low bias current, opamp. You connect the capacitor between the minus input and the output and bias the plus input to your reference (your reference is the voltage the reads half scale on your A/D). Now its itegrating the current going into the minus input. If you hook one end of a resistor to the minus input, it integrates the voltage (minus your reference voltage) on the other end of the resistor (scaled by the resistance value). 

To measure the voltage accross a motor (or leg if, brushless), you need to make a differential measurement so, you need to put an instrumentation amp or an opamp configured to be an instrumentation amp in front of your integrator. Currents are measured in three ways: a current sense transformer, shunt and AD8418 or simular, or hall effect current sensor. Current sense transformers connect between the buffered reference and the integrator through a resistor so you're measuring the voltage across the transformer which is proportional to the change in (derivative of) the current. The integral you get is the current and not the integral of the current which is cool but only works for AC currents. The AD8418 is basically an intrumentation amp and Hall effect sensors are usually instrumented so, they also connect through, just, a resistor and you get the integral of the current  at the output of the integrator, (input to the Teensy A/D) like you'd expect.

We've started what I'm calling "The Analog Board" on Jud's Github page: https://github.com/jud-mediumkahuna/
The first version of the skematic is hard drawn but we'll get it on KiCad soon (ditched Eagle) and there's a description of the circuit so please take a look. We plan to use this for a number of brain style control projects.






