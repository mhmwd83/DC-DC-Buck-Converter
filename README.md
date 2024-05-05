# <ins>Buck Converter Design </ins>


## Buck Converter Design
Before reading this section, please read the [Introduction](http://www.simonbramble.co.uk/dc_dc_converter_design/dc_dc_converter_design.htm).

All of the circuits in this tutorial can be simulated in LTspice®. If you are new to LTspice, please have a look at my LTspice Tutorial

In order to efficiently reduce a high voltage to a lower voltage, a buck dc/dc converter is needed.

Consider the circuit of FIG 1.

<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/07f1d989-2a3d-40a4-9845-ac19e6008b19">
</p> 

<h3 align="center"> 
  FIG 1.
</h3>

The top MOSFET switches on creating a short circuit between the input voltage (IN) and the left hand side of the inductor, L1. The inductor current ramps up according to the equation

<p align="center">
<img align="center" src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/225f0375-8c17-424e-b593-08932ac474ab">
</p> 

where V is the voltage <ins>across </ins> the inductor, L is the inductance value and di/dt is the <ins>change </ins> in current with time through the inductor. Thus with a fixed input voltage and a fixed output voltage, there is a fixed voltage across the inductor thus the change in current with time is constant (i.e. a ramp waveform).

The output voltage on startup is 0V, so the initial voltage across the inductor is equal to the input voltage. However, as the output voltage changes (and then reaches regulation) the above equation becomes.

<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/44d7378b-939f-486c-b9eb-472a5df7c635">
</p>

The peak inductor current is sensed by a small series resistor, R4, and when the voltage across this resistor equals a certain value (see the specific converter’s datasheet) the IC switches off the top MOSFET.

Now, inductors do not like having their current interrupted, so when the top MOSFET switches off, the inductor behaves like a battery to try to maintain the current flow. Referring to FIG 1, output side of the inductor tries to fly positive (to push current  out of the right hand side of the inductor) and its switched side (the left hand side) flies negative (to try and sink current into its left hand side) – in an effort to maintain the left to right current flow. Since the output side of the inductor is clamped by a capacitor, the left hand side flies negative. At this point the IC switches on the bottom MOSFET, Q2, to clamp the left hand side of the inductor to ground and enable the inductor to maintain its current flow.

Thus a current flows <ins>up </ins> MOSFET Q2, from left to right through the inductor and down into the output capacitor, thus charging the output capacitor.
 
When MOSFET Q2 switches on, it also provides a short circuit to 0V at the bottom of capacitor C6. Since the top of C6 is connected to the LTC3891’s internal linear regulator (INTVCC) via diode D1, this capacitor charges to INTVCC – 0.3V. The voltage on this capacitor is then used to provide a voltage higher than the input voltage to enable the top MOSFET to be switched on. Indeed, at startup, Q2 actually switches on before Q1 to charge the flying capacitor, C6, to enable Q1 to be switched on.

 The process then starts again, with Q1 switching back on again and recharging the inductor.

 The discharge cycle of the inductor is governed by the same equation as the charge cycle:

<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/5c6c75d3-8ec6-4c9c-8240-52c86b8d7463">
</p>

where V is the voltage across the inductor and is equal to the output voltage (since the left hand side of the inductor is clamped to 0V by MOSFET Q2).

The LTspice model of FIG 1 can be downloaded here (right click over the link and save as a '.asc' file): [LTC3854 buck converter](http://www.simonbramble.co.uk/dc_dc_converter_design/buck_converter/ltc3854_buck_converter.asc).

The datasheet of the LTC3854 can be downloaded here: [LTC3854 datasheet](http://www.linear.com/product/LTC3854).

Considering FIG1, the input voltage is 12V, the output voltage (in regulation) is 5V and the inductor value is 6uH

Thus from

<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/76f9eec1-c085-40f4-9174-21c19ff2a041">
</p>

we can determine that change in current when the inductor is charging is

<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/2e2961be-6f20-4bce-a35d-d7ae9e8675be">
</p>

or 1,166,666 Amps per second.
When the inductor is discharging (when Q2 is on), the inductor discharges according to the equation

<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/85aec801-e4eb-436a-a815-bff0c6482ac8">
</p>

or

<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/08d4831d-38b9-494c-b1b9-e50ddfdc0317">
</p>

which equates to 833,333 Amps per second.

<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/f655b470-a045-4c85-b845-ec76a566d687">
</p>

<h3 align="center"> 
  FIG 2.
</h3>

FIG 2 shows an LTspice simulation with the current ramping from 4.378A to 5.604A over 1.08us, a change of 1,135,185 Amps per second – not too far off what was calculated above.

During discharge, the current ramps down from 5.604A to 4.378A, but over 1.438us, a change of 852,573A – close to what we calculated.

The discrepancy in the above is because each MOSFET does not present a perfect short circuit and actually has about 50mV across it when fully activated.

It is interesting to note that the value of di/dt is determined ONLY by the inductance value and the voltage across the inductor. The controller IC has nothing to do with setting the inductor ramp current.

It is also useful to calculate the duty cycle of the converter. The duty cycle is the ratio of the ON time of the top MOSFET to the total period of oscillation.
The inductor charges according to

<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/08fec2d7-b5da-43eb-87df-85a27a7d64f2">
</p>

and discharges according to

<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/6e8468b8-21be-4a9b-a310-3fc049a59c30">
</p>

(here dt1 is the ON time of the top MOSFET and dt2 is the ON time of the bottom MOSFET)
In steady state, as can be seen in FIG 2 the charge current is equal to the discharge current, so

<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/a1b19d91-5099-4633-a512-5b4e2c92d748">
</p>

From this we can see that

<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/85fc654d-be07-4514-89ec-b730c3a6423b">
</p>

so
<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/1e21f85a-9da5-4d26-9c9a-894cb78e6ae4">
</p>

so
<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/88fb8443-fa0c-488b-b8f1-e558b228ffd3">
</p>

and if Duty Cycle (DC) is a ratio of dt1 to (dt1 +dt2) then

<p align="center">
<img src="https://github.com/mhmwd83/DC-DC-Buck-Converter/assets/96796504/49f1220b-56c2-48e8-ba77-02ed527d8ab8">
</p>

So the duty cycle is equal to the ratio of Vout to Vin.

It is interesting to note that the duty cycle is purely dependent on the input and output voltages and has nothing to do with the controller IC or inductor value.

The above is true as long as the inductor current does not fall to zero. The converter is then said to be operating in continuous conduction mode (CCM). If the inductor current ramps down to zero, the converter is then in discontinuous conduction mode (DCM).

In CCM if the load current changes, the duty cycle of the converter and the amplitude of the ripple current remain the same. The circuit responds to a change in load current by changing the midpoint of the inductor current (its dc offset). Indeed it is also true that the average inductor current in a buck converter is equal to the load current.

In FIG 2 we can see that the midpoint of the inductor current is 5A and it can be seen from FIG 1 that our load is 1 Ohm, thus the load current is 5A. If the load resistance were increased to 2 Ohms, the ripple current and duty cycle would remain unchanged (in the steady state), but the dc offset current would fall to 2.5A.



