# olympic-white
A switch-mode DC/DC converter module based on a TPS65131 to provide adjustable positive and negative rails from a 2.7V-5.5V supply.

Copyright (C) 2020 Lin Ke-Fong (anotherlin@gmail.com)\
*This project is free, you may do whatever you want with it.*

**_Draft: README.md will be updated after validation and debugging._**

## Purpose

For analog electronics, a bipolar supply is often needed, that is positive and negative rails like +15V and -15V.
Unfortunately, most of time, only a single positive supply is available, in particular, USB supplies a single +5V. 
The object of this switch-mode DC/DC module is to propose a workaround for that.

With an input supply between 2.7-5.5V, the TPS65130 and TPS65131 DC/DC converter ICs are perfectly suited for battery operated or USB 
devices. For example, my Solid State Logic SSL2 USB audio interface seems to make use of a TPS65130. A TPS65131 is used here to benefit 
from its higher current capability. 

The proposed module can be configured using 2 resistors and bridging 4 solder jumpers. It matches a DIP style footprint, hence it can be 
inserted in a DIP socket or used on a breadboard The previews should give you a better idea: [top](./figures/preview_top.png) and 
[bottom](./figures/preview_bottom.png)

## Usage

The TPS65131 implements a boost converter for the positive rail and a buck-boost (inverting) converter for negative.
Please refer to the [Texas Instrument's website](https://www.ti.com/product/TPS65131) for details, including the datasheet.
And then have a look at the [schematics](./figures/schematics.png).

Each rail features an "enable" and a "power save mode" pins. Solder jumpers **ENP** and **ENN** respectively control if positive or negative
outputs are enabled by external (bridge center and top pads) signals (pins **ENP** and **ENN** on **J2**) or always on (bridge center and 
bottom pads). Solder jumpers **PSN** and **PSP** control power save mode (refer to datasheet) for respectively positive and negative rails. 
Bridge center pad with top pad to turn-off or with bottom pad to turn-on. All solder jumpers must be appropriately bridged before use.

To program the positive output voltage, use the following formula: _R2 = (R1 * V_ref) / (V_pos - V_ref)_. Where _R1_ is 470k, 
_V_ref_ is 1.213V, and _V_pos_ is the desired output voltage. For example, for _V_pos = 12V_, we end-up with _R2 = 52851R_. We can take the 
standard resistor value 52.3k. Here are resistor values for some common positive output voltages:

| _V_pos_ | computed _R2_ | 1% resistor value |
| --- | --- | --- |
| 5V | 150543R | 150k |
| 9V | 73213R  | 73.2k |
| 10V | 64881R | 64.9k |
| 12V | 52851R | 52.3k |
| 15V | 41351R | 41.2k |

Note that 5V output can be achieved only if the supply is less than 4.5V.

To program the negative output voltage, use the following formula: _R4 = -(R3 * V_ref) / V_neg_. Where _R3_ is 470k, 
_V_ref_ is 1.213V, and _V_neg_ is the desired output voltage. For example, for _V_pos = -12V_, we obtain _R4 = 47509R_, hence we can take
the standard resistor value 47.5k. Here are resistor values for some common negative output voltages:

| _V_neg_ | computed _R2_ | 1% resistor value |
| --- | --- | --- |
| -5V | 114022R | 115k |
| -9V | 63345R  | 63.4k |
| -10V | 57011R | 57.6k |
| -12V | 47509R | 47.5k |
| -15V | 38007 | 38.3k |

For both programming resistors, use "low-noise" 1% metal film resistors. You may solder them on either (top or bottom) sides. 

## Fabrication

Some "ready to use" gerbers are available: [gerbers/olympic-white-gerber.zip](./gerbers/olympic-white-gerber.zip). The full Kicad 
project is available [kicad/olympic-white.zip](./kicad/olympic-white.zip). [JLCPCB](http://www.jlcpcb.com) is cheap efficient, 
be sure to order the stencil along. Then order the content of the [BOM](./documents/bom.csv).

## Remarks and Discussion.

* This is a lower level project and this documentation is probably not enough to make the most of it.

* Because the input supply is limited to 2.7-5.5V, this module can only works with battery operated or USB devices. This is unfortunate as 
quite a few electronic devices that could have benefit from such a module, operate at +9V (guitar effect pedals) or +12V. In retrospect,
the ADP5071 with its 2.85-15V input range and its wide -39V and +39V output ranges, would have been a better choice.

* For low-noise application, the outputs would probably need post-regulation (using a pair of LM2940 and LM2990 for example), or at least 
some carefuly designed additional filtering.

* Why _olympic-white_ ? This is the color of my Fender Jazz Bass and I figured out it would be a good project name.

## Revision history and status

* **_Version 0.8_**  Ready for fabrication and debugging/validation, upload on github.
  - Need to send PCB to fabrication;
  - Then do the assembly and debug/validate.
