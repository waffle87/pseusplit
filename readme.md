# Pseusplit

A pseudo-split keyboard with a trackpad or LCD display and per-key RGB

![pcb_top](img/pcb_top.svg)

![pcb_bottom](img/pcb_bottm.svg)

## Hardware Specifics
This keyboard uses an integrated STM32G0B1 microcontroller fit with a single push button switch^[1](#references)^, and USB-C. It also features a dedicated IS31FL3733 RGB Matrix driver to alleviate processing power from the primary MCU for LCD drawing, or other tasks.

## Ordering
The following instructions will be for [JLCPCB](jlcpcb.com), but should generally apply to most PCB manufacturers. The Gerber files in this repository include:
- [**pcb_gerbers.zip**](production/pcb_gerbers.zip): Main PCB
- [**plate_gerbers.zip**](production/plate_gerbers.zip): Switch Plate PCB^*^
- [**back_gerbers.zip**](production/back_gerbers.zip): Backplate PCB

* The switch plate PCB is designed for a single half of the keyboard (and is reversible), meaning ordering a quantity of 10 will be enough to cover 5 keyboards.

3. For PCB assembly, choose **Economic** PCBA type and **Bottom Side** for assembly side
4. Upload BOM file from [production/bom.csv](production/bom.csv) on the Bill of Materials page
5. Upload CPL file from [production/positions.csv](production/positions.csv) as well. Disregard any message like "J1 won't be assembled due to missing data". This is the display pin header
- Choose "File Provided as Complete File, proceed with my own files"
6. Select appropriate components on the following screen by typing in
the shown part number and select it. An effort has been made to choose
as few extended components as possible, however this is not possible
for some.
7. Have a quick look at component placement and make sure nothing
looks wildly wrong. It is normal for some components to be shown
oriented incorrectly or not quite where they belong. The PCB reviewers
will generally correct this without issue.

1. Upload the appropriate Gerber files described above
2. Set PCB quantity and colour as desired
3. For PCB assembly, choose **Economic** PCBA type and **Bottom Side** for assembly side
4. Upload the [BOM](production/bom.csv) and [CPL](production/positions.csv) files
5. Ensure the correct components have been selected, and all are in stock. An effort has been made to choose as few extended components as possible, but this is not possible for some.
6. Give a quick look at the component placement and make sure nothing looks wildly wrong. It is normal for some components to be shown oriented incorrectly or not quite where they belong. The PCB reviewers will usually correct this without issue.

### Additional Components
For optional components to be installed by hand:
| Item | Quantity | Remarks |
|------|----------|---------|
| 6028 RGB LEDs ([Alibaba]((https://www.alibaba.com/product-detail/smd-6028-RGB-led-diode-for_60637386884.html))) | 36 | Unforunately, LEDs matching this pinout are rather difficult to find, and are not carried by LCSC. |
| 4mm M2 Screws | 8 | Used for mounting PCB to plate case. Length does not have to be exact. |
| 6mm M2 Standoffs | 4 | Used for mounting PCB to plate case. Ensure maximum diameter is no greater than 4mm. |
| 240x240 LCD ([ST7789](https://www.adafruit.com/product/3787)) | 1^*^ | Other displays with same pinout and dimensions (no larger than 40x43mm) will work. |
| Azoteq Trackpad ([TPS43](https://www.mouser.com/ProductDetail/Azoteq/TPS43-201A-B?qs=HXFqYaX1Q2x%252BNkQCZlM0ug%3D%3D)) | 1^*^ | Installed via soldering wires to appropriate display header pins. |
| MX Switches | 36 | Pick your favourite switches! |

* Trackpad and LCD is either/or. Both cannot be installed at the same time.

## Assembly Instructions
These steps assume you already have an assembled PCB and are installing switches, trackpad, etc. If you are hand-assembling the PCBs with all components, the [interactive BOM](https://html-preview.github.io/?url=https://github.com/waffle87/pseusplit/blob/master/ibom.html) will be handy.

1. **Compile and flash firmware.** This assumes a local QMK build environment has already been setup. Firmware sources can be found [here](https://git.pngu.org/qmk_me/keyboards/pseusplit). Enter bootloader by holding the reset button while plugging PCB in. Flash firmware using `qmk flash` or QMK Toolbox.
2. **Ensure the keyboard is functional** by shorting switch footprint pins with metal tweezers. This should output keypresses on the computer.
3. **Install LCD display if desired.** Solder the 12 pin header to the labeled footprint on the PCB and solder display to the header. In the firmware, enable the Quantum Painter feature and disable the Pointing Device feature if necessary.
4. **Install Azoteq trackpad if desired.** Solder wires from the pins on the trackpad to the appropriate pins on the 12 pin header. Refer to schematic for specifics. In the firmware, enable the Pointing Device feature and disable the Quantum Painter feature if necessary.
5. **Install switches** by inserting a few switches into the switch plate (corners/middle), and then solder to PCB. Fill in rest of switches, testing as you go.
6. **Assemble plate case**. Insert the M2 screws into the back plate and affix standoffs to screws. Place PCB + Plate onto standoffs and install remaining 4 screws.

## References
1. [A single-push reset circuit for STM32](https://acheronproject.com/reset_article_1/reset_article_1)
