# strayr-k-macros
Some useful macros for Klipper

## WARNING
- DO NOT just copy one macro from here without understanding it and expect it to work, it likely calls other macros within this suite to avoid code duplication. I made extensive use of the `[save_variables]` functionality in lieu of a good config file.

## Alternatives
- MainsailOS ships with this config that uses the same pause/park mechanic, if you just want that then maybe look there instead? https://github.com/mainsail-crew/MainsailOS/blob/master/src/modules/mainsail/filesystem/home/pi/klipper_config/mainsail.cfg


## Features

- Intelligent pause/park system that determines a safe park position from the toolhead height and your config.
- Automated load/unload/M600 system that uses a few simple measurements stored in a separate file that isn't hosed when you update
- Mesh and bed offset helper macros that make printing at different temperatures and swapping build surfaces fast and painless. Just tell the printer which bed you put in or if you want ot rebuild the mesh
- Musical M300 system that informs you of the state of the printer by audio cue
- Optional M204 replacement that treats acceleration settings defined in your config as hard limits **This can cause issues if you have a lot of acceleration changes close together** I've noticed this printing fine detail with a lot of z-hops on the first layer, wherein every z-hop hoest though 1st layer and travel acceleration settings in both directions, and some slicers are smart enough to set these accelerations anyway even when there is no change. ~~I think it's an issue with `ACCEL_TO_DECEL` making huge changes, but I think it's better to set one acceleration that looks good in klipper and leave it.~~ Orca slicer shows that setting ACCEL_TO_DECEL doesn't cause the same problem, it looks like it was repeated macro calls that were hurting.
- Prusa style G34 that can tram a dual-stepper single driver gantry, requires UART/SPI control of stepper driver current and some tuning.
- Braindead manual levelling of a dual stepper gantry
- Optional M84 and idle timeout that allows you to shut off all steppers except the Z so you don't lose your gantry level and gave to keep repeating G34
- Optional slimmed down menu
- setup_macros.cfg aliases voron style PRINT_START to START_PRINT and checks for voron style parameters as well as klipper style giving more
flexability with gcode generators. 
- Extensive bed offset management, can input new beds from the console and can auto adjust all beds after a nozzle change.

## Installation

- Clone this repo into the settings folder and include the functionality you want

```
git clone https://github.com/strayr/strayr-k-macros.git
```

Add this to printer.cfg
```
[include strayr-k-macros/common.cfg]
[save_variables]
#set this filename and path to somewhere convenient. Assumes your config lives in ~/klipper_config, it might be in ~/ or elsewhere
filename: ~/kprint-conf/printer_variables.cfg

[include strayr-k-macros/setup_macros.cfg] # this includes start print, cancel print etc, intended for bedslingers, if you need changes, copy it to your config folder and include your local version instead.
```

You will need to need to copy the following files into you your settings directory and make changes:
```
[include beeper.cfg] # requires beeper pin setting
[include save_variables.cfg] # lots of printer specific settings.

```

The following enable optional functionality, add to your printer.cfg as required
```
[include strayr-k-macros/mechanical_level_tmc2209.cfg] # provides G34 gantry level. Read before use. 
[include strayr-k-macros/idle_timeout.cfg] # use on printers with a z that falls when unpowered
[include strayr-k-macros/extrusion_test.cfg] # contains some macros to calibrate flow
[include  bed_offset.cfg] # read and understand this
[include k-macros/extrusion_test.cfg] # totally optional
```

once you have the macros in place you with to use, run the follow extended gcode:
```
FIRMWARE_RESTART
_INIT_ALL
```

## Interdependent Code
- I plan on making some of the simpler features standalone single file includes rather than a big web of code. This will likely be tagged version 0.2 and `save_variables.cfg`'s version checks will be incremented to deliberatey make you check things over.
- We really do need a way of saving some state so `save_variables.cfg` isn't going away.


## Mechanical Gantry Calibration

- please read the comments at the top of each file before use.
- `mechanical_level_tmc2209.cfg` requires the capability to set stepper currents with SET_TMC_CURENT, and may need additional physical endstops. Improper use may cause **expensive damage**. It's in use on my Ender 3 derived printer but please read the comments at the top of the file and watch the video before use. **You have been warned** https://www.youtube.com/watch?v=aVdIeIIpUAk
- `mechanical_level_stupid.cfg` is a bad idea and shoud not be used unless you've read it, understood it, know it can break your printer and are willing to do so. JUST DON'T.

## Bed Offset

This is relatively stand alone, you need to set up [save_variables] and add a line to your START_PRINT and END_PRINT macros.

You may benefit from having the PROBE_CALIBRATE menu items in improved_menu.cfg set up so you can run all that from the screen.