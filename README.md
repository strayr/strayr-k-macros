# k-macros
Some useful macros for Klipper

- Clone the folder into the settings folder and include the functionality you want
  ```
  [include k-macros/common_functions.cfg] # used everywhere
  [include k-macros/pause_park.cfg]
  [include k-macros/M300.cfg]
  ```
  You will need to copy the following into you your settings directory (or straight into your settings file) and make changes:
  ```
  [include beeper.cfg] # requires beeper pin setting
  ```
- please read the comments at the top of each file before use.
- `mechanical_level_tmc2209.cfg` requires the capability to set stepper currents with SET_TMC_CURENT, and may need additional physical endstops. Improper use may cause **expensive damage**. It's in use on my Ender 3 derived printer but please read the comments at the top of the file and watch the video before use. **You have been warned** https://www.youtube.com/watch?v=aVdIeIIpUAk