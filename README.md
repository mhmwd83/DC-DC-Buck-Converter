# Buck Converter Design
## Buck Converter Design
Before reading this section, please read the [Introduction](http://www.simonbramble.co.uk/dc_dc_converter_design/dc_dc_converter_design.htm).

All of the circuits in this tutorial can be simulated in LTspice®. If you are new to LTspice, please have a look at my LTspice Tutorial

In order to efficiently reduce a high voltage to a lower voltage, a buck dc/dc converter is needed.

Consider the circuit of FIG 1.

<img height = "500" width="250" align="center" src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/07f1d989-2a3d-40a4-9845-ac19e6008b19">

<h3 align="center"> 
  FIG 1.
</h3>

The top MOSFET switches on creating a short circuit between the input voltage (IN) and the left hand side of the inductor, L1. The inductor current ramps up according to the equation

![image](https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/225f0375-8c17-424e-b593-08932ac474ab)

where V is the voltage ***across*** the inductor, L is the inductance value and di/dt is the ***change*** in current with time through the inductor. Thus with a fixed input voltage and a fixed output voltage, there is a fixed voltage across the inductor thus the change in current with time is constant (i.e. a ramp waveform).

 

The output voltage on startup is 0V, so the initial voltage across the inductor is equal to the input voltage. However, as the output voltage changes (and then reaches regulation) the above equation becomes

 



 

 

The peak inductor current is sensed by a small series resistor, R4, and when the voltage across this resistor equals a certain value (see the specific converter’s datasheet) the IC switches off the top MOSFET.

 

Now, inductors do not like having their current interrupted, so when the top MOSFET switches off, the inductor behaves like a battery to try to maintain the current flow. Referring to FIG 1, output side of the inductor tries to fly positive (to push current  out of the right hand side of the inductor) and its switched side (the left hand side) flies negative (to try and sink current into its left hand side) – in an effort to maintain the left to right current flow. Since the output side of the inductor is clamped by a capacitor, the left hand side flies negative. At this point the IC switches on the bottom MOSFET, Q2, to clamp the left hand side of the inductor to ground and enable the inductor to maintain its current flow.

 

Thus a current flows up MOSFET Q2, from left to right through the inductor and down into the output capacitor, thus charging the output capacitor.

 

When MOSFET Q2 switches on, it also provides a short circuit to 0V at the bottom of capacitor C6. Since the top of C6 is connected to the LTC3891’s internal linear regulator (INTVCC) via diode D1, this capacitor charges to INTVCC – 0.3V. The voltage on this capacitor is then used to provide a voltage higher than the input voltage to enable the top MOSFET to be switched on. Indeed, at startup, Q2 actually switches on before Q1 to charge the flying capacitor, C6, to enable Q1 to be switched on.

 

The process then starts again, with Q1 switching back on again and recharging the inductor.

 

The discharge cycle of the inductor is governed by the same equation as the charge cycle:

 



 

where V is the voltage across the inductor and is equal to the output voltage (since the left hand side of the inductor is clamped to 0V by MOSFET Q2)


