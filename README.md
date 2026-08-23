# UHF Modulator ALPS MDLP3W104A
<img align="left"  src="/pictures/modulator_alps_mdlp3w104a_pollin_605067.jpg">

Some Reverse Engineering of the UHF-Modulator Type MDLP3W104A made by ALPS which can be cheaply obtained from Pollin 
(Artikel-Nr.: [605067](https://www.pollin.de/p/alps-uhf-modulator-mdlp3w104a-605067)).
This Modulator contains the IC **Motorola MC44353**.
<br clear="left">


## Abstract
The used PLL IC MC44353 can be programmed in 250 kHz steps within a register range from 4.25 MHz to 1023.75 MHz (assuming a 4 MHz XTAL) which makes it quite interesting also for Ham-Radio tinkering.

The modulator itself is advertized as a multistandard video modulator for the UHF band speciefied covering [channels E21 to E69](https://en.wikipedia.org/wiki/Television_channel_frequencies#Western_Europe,_Greenland,_most_countries_in_Asia_and_Africa,_and_most_of_Oceania) (471.250 MHz to 855.250 MHz) but investigations showed that it can cover the 70 cm Amateur Radio Band (430 to 440 MHz) and can go even as **low as Channel S23** (319.250 MHz in the German tv-cable networks so called [Hyperband](https://en.wikipedia.org/wiki/European_cable_television_frequencies#Hyperband)).
The upper end reaches at least channel E73 (887.250 MHz, not allocated to TV broadcast!) with 28 V tuning voltage tested and may even exceed with higher tuning voltage applied (33 V max!).


## Datasheets & Schematics
* Datasheet & Schematic Modulator: [ALPS MDLP3W104A](datasheet/alps_mdlp3w104a_1997.pdf)
* Datasheet "PLL Tuned UHF Audio/Video Modulator ICs for PAL, SECAM and NTSC TV Systems": [Motorola MC44353](datasheet/motorola_mc44353_mc44354_mc44355_1998.pdf)


## Controller Firmware
A first Arduino-Firmware using an ESP8266 allowing stepping through all channels has been presented here: [TV Channel Controller for MC44353 on ESP8266](https://github.com/DB1BMN/TV_Channel_Controller_MC44353)


## Pictures
<img width="640" src="pictures/UHF-Modulator_ALPS_MDLP3W104A_Ref_Designator_2026-08-22.jpg">

## Pitfalls and Caveats
### Supply
* The supply voltage of the modulator anlog and digital part is **+5 V +/- 0.3 V** @ 90 mA max.
*  **Negative** supply voltage must be applied via **chassis ground**. There is no dedicated GND-pin on the pin header!
*  The Antenna "Booster" i.e. a feed-thru-amplifier from the aerial can be separately supplied with 5 V / 45 mA if needed.
*  The tuning voltage **TUNING B+** for the VariCap diodes shall be in the range of **30 V +/- 3 V**. The typical current draw from this pin is specified with 0.1 mA

### I2C-Bus
The I2C-Bus needs **pull-up resistors** from +5 V do each data line.

When using a 3.3 V microcontroller a level shifter might be necessary.
On the modulator PCB the I2C data lines are equipped with 270 Ohm series termination resistors. 
This might bring additional challanges since the 
**SDA line is "weak"**
sliding up to 1 V when sinking 3 mA in "Low" state. 
Those reistors will add additional 270 mV per mA of voltage drop which can cause an invalid state during **ACK transfer**.
E.g. the ESP8266 accepts only 25% of its supply voltag as a valid "Low" level i.e. 825 mV when powered from 3.3 V!

FIXME: screenshot of data transfer and ACK

A value of **3.3 kOhm** seems to be good starting point.

Whe problems occur consider using an active bus driver or an isolator (e. g. [ISO1540](https://www.ti.com/product/de-de/ISO1540)).

### PLL Locking Indicator
Since the MC44353 has neither a status bit nor pin for the PLL locking status you will have to evaluate the VCO voltage to obtain this information if needed.

An operational amplifier used as a voltage follower is recommended since the VCO voltage is quite high impedance (560k). It should have low bias current, high input voltage (33 V!) and sufficient low input capacitance not to decrease bandwidth when observing step response (the phase detector is generating pulses with 31.25 kHz!). 
The [ADA4511-2](https://www.analog.com/en/products/ada4511-2.html) looks like a promising candidate.

For a quick PASS/FAIL test the DC voltage can be observed with an multimeter since the control loop will maintain the correct VCO DC voltage across the VariCap diode as long the PLL is in lock state.

### "Multistandard"
The MC44353 is advertized as a "multistandard" modulator IC but it should be rather called 
**[multisystem](https://en.wikipedia.org/wiki/Broadcast_television_systems#ITU_standards)** 
since it can not generate a signal according to certain [color standard](https://en.wikipedia.org/wiki/Color_television#Color_standards) on it self!
It can only be set up to certain modulation parameters (sound preemphasis and offset, modulation depth etc.) required by these standards.

You will have to provide a proper baseband video signal in [CVBS-format](https://en.wikipedia.org/wiki/Composite_video) from an analog source like a DVD-Player or
Raspberry Pi's AV-out. -- Rumor has it that older versions of the Pi (up to V4) can generate a 
[Teletext](https://en.wikipedia.org/wiki/Teletext) signal 
(in Germany called Videotext ). 
Check out
[raspi-teletext](https://github.com/ali1234/raspi-teletext)
and
[VBIT2](https://github.com/peterkvt80/vbit2)
for more details!
