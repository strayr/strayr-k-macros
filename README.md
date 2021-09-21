# k-macros
Some useful macros for Klipper

- Clone the folder into the settings folder and include the functionality you want
  ```
# This brings in toys that don't break things
[include strayr-k-macros/common.cfg]

## Include the below explicity
# 
[include strayr-k-macros/mechanical_level_tmc2209.cfg] # provides G34 gantry level. Read before use. 
[include strayr-k-macros/idle_timeout.cfg] # use on printers with a z that falls when unpowered
[include strayr-k-macros/extrusion_test.cfg] # use 
[include strayr-k-macros/setup_macros.cfg]
  ```
  You will need to copy the following into you your settings directory (or straight into your settings file) and make changes:
  ```
  [include beeper.cfg] # requires beeper pin setting
  [include save_variables.cfg] # lots of printer specific settings.
  ```
- please read the comments at the top of each file before use.
- `mechanical_level_tmc2209.cfg` requires the capability to set stepper currents with SET_TMC_CURENT, and may need additional physical endstops. Improper use may cause **expensive damage**. It's in use on my Ender 3 derived printer but please read the comments at the top of the file and watch the video before use. **You have been warned** https://www.youtube.com/watch?v=aVdIeIIpUAk
- `mechanical_level_stupid.cfg` is a bad idea and shoud not be used unless you've read it, understood it, know it can break your printer and are willing to do so. JUST DON'T.