The limit switches are used to detect the physical limits of the working area and to position the head in initial position during the homing process. Properly connected limit switches can increase significantly the reliability of the GRBL - the microcontroller pins connected to the switches are very vulnerable to any noise. 

Before starting, make sure your coordinate frame is setup properly on your CNC machine and satisfies the right-hand rule. If you're not sure, its explained in the quick setup guide [here](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Configuration#quick-guide-to-setting-up-your-machine-for-the-first-time). Otherwise, you will likely encounter problems with the homing cycle, where it behaves strangely. If you are having issues with the homing cycle, read this [FAQ](https://github.com/gnea/grbl/wiki/Frequently-Asked-Questions#homing-cycle-isnt-working-right-the-movements-are-all-going-in-wrong-directions).

There are two types of end switches wiring:
- Normally Opened end switches (NO) - switches are connected in parallel, if the head hits one of the switches the resistance becomes low (<10 Ohm). The wiring is simple but there is no indication if one of the switches is disconnected (broken wire).

- Normally Closed end switches (NC) - switches are connected in serial, if the head hits one of the switches the resistance become high (> 1 MOhm). The wiring is more complicated but if any of the switches is disconnected (broken wire) this will be immediately detected. This is the way how all professional CNC machines end switches were wired.  

The easiest way to attach limit switches to Arduino UNO is to just connect the switches to the corresponding pins and to rely on the internal weak pull up resistors (~47K) of the ATMega328 chip. The Normal connected (NC) switch wiring is shown below:

![Normal Closed switches](https://cloud.githubusercontent.com/assets/5912573/22624947/4abbfa48-eb92-11e6-8b16-5fff7d2a6a8f.png)

----


The Normal Open (NO) switch wiring is shown below:

![Normally Open](https://cloud.githubusercontent.com/assets/5912573/22624880/1b605600-eb91-11e6-9a87-54a98b1cb38c.png)

One improvement is to connect 1K to 4.7K pull up resistors to 5V and 100nF capacitors to GND. The extra pull ups and capacitors have noticeable noise suppression effect over the GRBL performance.

----

![NC Filtered](https://cloud.githubusercontent.com/assets/5912573/22625452/1671414a-eba0-11e6-9fb1-648a82bd19bf.png)

----

![1a953482-eba2-11e6-9e60-08926e41fe0f_fix](https://user-images.githubusercontent.com/1461231/34397640-bbb8628c-eb45-11e7-91ca-f53a38e70566.png)

----


Adding shielded cables to the end sensors or at least using twisted pair of two wires reduces further the noises injected from the next door stepper motor cables.

The ultimate solution for noise filtering on end switches is to add optocouplers - they have many benefits compared to the listed above solutions:
- There is no direct galvanic connection between the end sensor and the microcontroller pin - any ESD discharges will not affect the GRBL controller
- Optocouplers are inert elements - short glitches will simply not pass at all
- Optocouplers are current driven elements and they require huge energy from the noises in order to pass - in normal operating conditions they effectively cut all the noises  


----

If you're building a two-axis system, such as a laser engraver or pen plotter, you may also wish to read the limit switch section of the [Two-Axis System Considerations page](https://github.com/gnea/grbl/wiki/Two-Axis-System-Considerations).

----


During the [discussion on GRBL forum](https://github.com/gnea/grbl/issues/96) we came to the following design of GRBL limit switch end sensor break out board - see the images below. The board is single side PCB (1.0mm to 1.6mm FR4) and uses connector with screws for attaching the end sensor wires. We recommend to use crimping of the wires before inserting them into the connectors.


----

The schematic of the end sensor board which uses optocouplers

The LIMIT SWITCH side and the ARDUINO side should use 2 different isolated power supply to take real advantage of the opto isolation.

BEST
![opto_limit](https://user-images.githubusercontent.com/1461231/36128580-1c40fc8a-1031-11e8-9269-4489a7f49fbe.jpg)

OK
![Schematic](https://cloud.githubusercontent.com/assets/5912573/22625815/7640a26c-eba7-11e6-9a5a-7d5e521488d8.jpg)

----

The BOM file of used components and the estimated price of the board
![BOM](https://cloud.githubusercontent.com/assets/5912573/22625825/a0426e4c-eba7-11e6-98d0-b9f09df72e89.jpg)

----

The 3D view of the board - this is how it will look like after assembly.
![Assembly](https://cloud.githubusercontent.com/assets/5912573/22625834/cd2eae48-eba7-11e6-9ec0-887fec41f2c7.jpg)

----

The bottom copper layer (viewed from top side of the board)
![Bottom Copper Layer](https://cloud.githubusercontent.com/assets/5912573/22625856/926e2076-eba8-11e6-9e95-0e758be23097.jpg)

----

The top silkscreen layer (viewed from top side of the board)
![Silkscreen Layer](https://cloud.githubusercontent.com/assets/5912573/22625869/dd9c9e92-eba8-11e6-8d64-644fa1b57167.jpg)

----

The bottom solder resist layer (viewed from top side of the board)
![Solder Resist Layer](https://cloud.githubusercontent.com/assets/5912573/22625870/e12a98e8-eba8-11e6-86b1-05590bbe71ea.jpg)

----

The DXF file of the copper layer (view from bottom - ready for milling)
[GRBL.DXF.V2.zip](https://github.com/gnea/grbl/files/752920/GRBL.DXF.V2.zip)

----

The GCode generated in CopperCam for milling the board - by having some CNC and engraving bit it's possible to mill the board from FR4.

[Simple.GRBL.zip](https://github.com/gnea/grbl/files/752921/Simple.GRBL.zip)

----

The gerber files and the NC drill files for producing properly the PCB (you can send these files to any PCB factory to get good quality boards).

[Gerbers.zip](https://github.com/gnea/grbl/files/752923/Gerbers.zip)

----

The schematic/BOM/PCBs and other layer images as single PDF for convenient documentation

[End.switches.break.out.board.pdf](End.switches.break.out.board.pdf)

----


For [Woodpecker PCB](https://aliexpress.com/store/product/GRBL-0-9J-USB-port-cnc-engraving-machine-control-board-3-axis-control-laser-engraving-machine/1941516_32713561151.html?detailNewVersion=&categoryId=4338) the wiring of end switches should be done in the following way:

![Woodpecker PCB](https://cloud.githubusercontent.com/assets/5912573/22929087/89c715ce-f2c2-11e6-9ffc-6f078f510d74.jpg)

for Woodpecker PCB v3.4 (the black one) :
Silkscreen labels for X and Z limit switches on newly released (actually january 2020)Woodpecker 3.4 CNC control board appear to be wrong ( reversed )

----------------
You can use some cable housing and crimp the wires to get more professional look (these housing connectors will fit straight into the board header)

http://uk.farnell.com/multicomp/2226a-02/crimp-housing-1-row-2-way/dp/1593506


----------------

# Normally-Closed (NC) vs Normally-Open (NO) switches

> This is the way how all professional CNC machines end switches were wired.

Here's why:

Use NC switches. Because whenever switches fail, the failure mode is ALWAYS "open" or "fails to make contact". Simple fact of nature, it's just the way things are.

Assuming a NO switch is used..... While homing in on a defective switch, you will not know that the switch is malfunctioning, not until after you have crashed your machine. If the switch fails to make contact then the machine crashes.

Assuming a NC switch is used..... If the switch is bad (in this case the contacts will be open), then no homing occurs, GRBL will return an error and homing does not proceed, and your machine doesn't crash.

So it does make a BIG difference which switch contact configuration you use for limit switches, NC or NO. Crash or no crash.

## Capacitors for noise filtering

Even if you do use NC contacts, you still need those 104 (0.1uF) capacitors, as close to the Arduino as you can place them. You can argue all day that those caps won't make a difference since the caps are shorted out by the switches. The explanation for this phenomenon is quite long but the first power line glitch will convince you otherwise. (Plug in your blender next to the CNC's AC plug and turn it on. Your CNC should still behave normally despite the blender.)

Side benefit: With NC switches, the connection is broken cleanly when you hit home position, therefore no contact bounce occurs. (Contact bounce occurs only during switch closure, NOT during switch opening.)

