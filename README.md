## York Modular 'Bad Trip' VCO

**This module is now discontinued - there will be no more made unless there is sufficient demand to justify a new run of boards and panels.**

**Information provided here is provided under the terms of the Creative Commons License Attribution-ShareAlike 4.0 International**

These are the files that you may (or may not) need for the Bad Trip VCO kit. It is assumed that you have enough knowledge to source appropriate components and read a schematic. 

Knowledge of how to use the Arduino IDE (or equivalent) to flash an ATTiny45/85 MCU is also desireable but not essential. In order to flash the firmware, your Arduino setup will need to include the ATTiny core by David Mellis - you can download that at https://github.com/damellis/attiny or install it via the Board Manager on newer versions of the Arduino IDE. As is, the default firmware will fit onto an ATTiny45 with space to spare - when flashing the firmware you'll need to specify the appropriate MCU with a 16MHz internal clock. Whilst there's absolutely no reason why you can't use a different clock speed, the frequency lookup table values assume a clock speed of 16MHz

The 'Bad Trip' is a square/saw VCO with a built in resonant vactrol filter - it gets it's name from the fact that you can coax some pretty squonky acid-style basslines out of it on the one hand, and utterly godawful nightmare noises on the other. 

More fun than it sounds, really.

Files included are:

- `badtrip-schematic.png` - the board schematic.
- `badtrip-bom.xlsx` - the Bill of Materials (BOM); this is a list of all the bits you'll need to complete the build.
- The `panels` directory contains design files for the front panel in AutoCAD (`.dxf`) and Schaeffer (`.fpd`) formats. You'll need these if you plan to fab your own panels.
- The `src` directory contains the firmware source file and a corresponding `.hex` file.
- `gerbers` contains the Gerber file for fabrication. These are what you want to be sending to the fab.

The firmware has a permissive license, so feel free to hack on it - all pull requests will be considered.

### Construction

There are no particular gotchas in terms of construction, however you should consider the following when setting the module up:

- Trimmer R7 controls the amount of current flowing into the two filter vactrols.
- Trimmers R10 and R11 affect the 'maximum' resistance of the two filter vactrols. They don't necessarily need to be trimmed to the same value - indeed, you'll get more interesting sounds out if they aren't.
- The effect of the feedback resistor will depend on the values of R10 and R11; in a lot of cases the response is somewhat 'digital' - if you're building from scratch then you can replace this pot with a wire link (and make sure you remove the corresponding hole from the panel)

### IMPORTANT

Since the VCO is microcontroller-based there are limits to the range of CV voltages that can be handled - anything negative is 'rejected' (ie. treated as 0V) whereas anything above 5V is clipped to 5V - the net result is that the VCO has a range of 5 octaves. Under most circumstances you'll want to have the CV pot fully clockwise, however if you're outputting particularly 'hot' CVs then you'll need to fiddle with the CV attenuator to bring the incoming CV down to something that the MCU can handle.

By default, the lowest note output by the VCO is C1 (32Hz) - you can alter this in firmware by modifying the frequency lookup table.

The tracking is pretty decent over 5 octaves but suffers a little due to the resolution of the ATTiny's ADC (10 bits) - this means that the input range is split into 1024 'divisions', with each division corresponding to around 4mV. If you want super-accurate to-the-cent tuning then you might well be disappointed, although you could probably fix things up in the firmware to a certain extent - this is where most of the work is done anyway.