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




