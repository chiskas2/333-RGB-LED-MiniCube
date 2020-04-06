# 333-RGB-LED-MiniCube

DISCLAIMER:
    This is provided on an as-is basis
    This is for a 3x3x3 RGB LED cube (so "somewhat limited" resolution..)
    The aim was (is?) an 8x8x8 cube.
    Some things are left unfinished, some unneccerasy bits included.

    It's an indefinitely paused project...

NOTE:
   You need to put all files in a folder of the same name as the main .ino file (without the .ino extension).
   Namely "333_RGB_LED_minicube_30_Release_1".


SCHEMATICS DESCRIPTION


  No schematics atm but in short: 
  Directly driven by and connected to an Arduino Nano (or compatible).

  Each color plane MUST HAVE AN ASSOIATED SERIES RESISTOR !!
  The value was chosen by experimentation, and is what my RGB LEDs seemed to be the most white with
  (with max intensity on R,G and B channels).
  
     ---> NOTE! Keep the total current for one row (3 RGB LEDs as white) within Arduino specs for one pin (the row pin)! <---

  9 rows of 3 RGB LEDs each, organized as 3 by 3 rows in a plane, as seen from the side.
  A specific RGB LED is lit by intersection between a row and a plane (or column).

  One row is lit at a time, for a maximum of 1/9th intensity.

  Rows    : are along the X axis (width)
  Columns : are planes along Y-Z axis really (I did call it column in code.. I meant plane!)


  Arduino digital output ("pin") - to - RGB LED (Common cathode) mapping:
 
    0,3,6 (PortD)       = blue planes  ( 2.7 k ohm series resistors )
    1,4,7 (PortD)       = green planes ( 2.7 k ohm series resistors )
    2,5,8 (portD/B)     = red planes   ( 470 ohm series resistors )
    9-17                = row pins (active-low)
                          (pin 9 = row 0 upper front, pin 17 = row 8 lower back, see below)
    18                  = Selection button (active-low)
    A7 (Analog input 7) = mic (from preamp)


    PIN - TO - ROW position mappings right side view:

                 Top    Digital outputs/pins
                       /
              9  12  15
     Front   10  13  16   Back
             11  14  17
     ----------------------------  <-- Base (PCB / circuit board)


     Another way to look at it:

     Isometric side view (Row pins and row numbers indicated on the "Right side")

        
                                 Left side
          
                            _ X axis (width)
                            /|
                           / ____________
                            /   /   /   /|
                         0 /___/___/___/ | 2
                          /   /Top/   /| |
                       1 /___/___/___/ |/| 1
                        /   /   /   /| | |
        Front        2 /___/___/___/ |/|/| 0      Back
                pin    | 9 | 12| 15| | | |
               row     |0__|3__|6__|/|/|/  |
                pin    | 10| 13| 16| | /  \|/
               row     |1__|4__|7__|/|/    
                pin    | 11| 14| 17| /     Y axis (heigth)
               row     |2__|5__|8__|/
                      
                         0   1   2  ---> Z axis (depth)
                
                      Right side



