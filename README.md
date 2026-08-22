# UHF Modulator ALPS MDLP3W104A
<img align="left"  src="/pictures/modulator_alps_mdlp3w104a_pollin_605067.jpg">

Some Reverse Engineering of the UHF-Modulator Type MDLP3W104A made by ALPS which can be cheaply obtained from Pollin 
(Artikel-Nr.: [605067](https://www.pollin.de/p/alps-uhf-modulator-mdlp3w104a-605067)).
This Modulator contains the IC **Motorola MC44353**.
<br clear="left">


## Abstract
The used PLL IC MC44353 can be programmed in 250 kHz steps within a register range from 4,25 MHz to 1023,75 MHz (assuming a 4 MHz XTAL) which makes it quite interesting also for Ham-Radio tinkering.

The modulator itself is advertized as a multistandard video modulator for the UHF band speciefied covering [channels E21 to E69](https://en.wikipedia.org/wiki/Television_channel_frequencies#Western_Europe,_Greenland,_most_countries_in_Asia_and_Africa,_and_most_of_Oceania) (471,250 MHz to 855,250 MHz) but investigations showed that it can cover the 70 cm Amateur Radio Band (430 to 440 MHz) and can go even as **low as Channel S23** (319,250 MHz in the German tv-cable networks so called [Hyperband](https://en.wikipedia.org/wiki/European_cable_television_frequencies#Hyperband)).
The upper end reaches at least channel E73 (887,250 MHz, not allocated to TV broadcast!) with 28 V tuning voltage tested and may even exceed with higher tuning voltage applied (33 V max!).


## Datasheets & Schematics
* Datasheet & Schematic Modulator: [ALPS MDLP3W104A](datasheet/alps_mdlp3w104a_1997.pdf)
* Datasheet "PLL Tuned UHF Audio/Video Modulator ICs for PAL, SECAM and NTSC TV Systems": [Motorola MC44353](datasheet/motorola_mc44353_mc44354_mc44355_1998.pdf)

## Pictures
<img width="640" src="pictures/UHF-Modulator_ALPS_MDLP3W104A_Ref_Designator_2026-08-22.jpg">

## Controller Software
TBD
