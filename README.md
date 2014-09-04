tidetank
========

An Arduino-based system to coordinate a fish tank's water level with the actual local tide.
 
This sketch runs on an Arduino Uno with an Ethernet shield as well as an Adafruit RGB LCDshield on top.
 A potentiometer is connected to A0 and mounted on the side of the tank; a float on a long stick 
 turns the potentiometer towards clockwise when the water's higher and towards counterclockwise 
 when it's lower.
 A servomotor (operating a drain valve) is connected to pin 8.
 
 Our fishtank is constantly being filled from a sump into which it drains. Changing the rate
 of drainage out of the tank will dynamically change how full it is at any given moment. The
 present system achieves desired water tank height by adjusting the drain flow rate.
 
 The basic program flow is:
 
 1) load saved calibration points from EEPROM, start systems up
 2) go online and request current water height data from NOAA
 3) determine how high the water in the tank should be, as a percent
 4) increment or decrement the drain valve by small amounts until the water is at the correct hight.
 
 Step 2 repeats every 6 minutes (NOAA updates their data at that interval).
 If Step 2 fails, it repeats every 10 seconds, trying to get back online.
 Steps 3 and 4 repeat every 5 seconds.
 
 There are also buttons implemented for various purposes:
 * Left opens the drain and halts the program.
 * Right closes the drain and halts the program.
 * Up sets the current float sensor position as 100% (and saves to EEPROM)
 * Down sets the current float sensor position as 0% (and saves to EEPROM)
 * Select immediately requests a web update, instead of waiting the usual 6 minutes.
 
 Written by Robert Zacharias for/at Rocking the Boat, http://www.rockingtheboat.org , an awesome
 nonprofit organization in the South Bronx, New York City. Feel free to contact the author with 
 questions/comments/etc. at bobby.zacharias@gmail.com.
 
 v. 1.0 completed on 8/27/14
 
 edits:
 v. 1.0.1b, 8/29/14 revision: 
   changed servo default to closed
   changed so valve closes always in big bites (our tank drains much faster than it fills)
   write valve closed at top of setup() so valve always closes first
   fixed valveOpen and valveClosed as per experimental outcomes
   restated adjustValvePosition() in terms of valveOpen and valveClosed rather than absolute values like 0 and 180. 
 
 v. 1.1, to be done:
  fix water height readings--need to be made from an arc path into a linearized height reading
  (probably write a function for this as it's a few steps in a chain)
  (also this might require a more complex zeroing process, probably putting the valve arm straight up,
   straight horizontal, and then finally calibrating to high and low water levels, so the trig can
    know what angles are represented by each position and do the height math appropriately) 
 
 incorporates material from Michael Margolis's "Requesting Data from a Web Server Using XML" https://www.inkling.com/read/arduino-cookbook-michael-margolis-2nd/chapter-15/recipe-15-5
 
