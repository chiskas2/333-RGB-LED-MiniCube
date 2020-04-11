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

  Directly driven by and connected to an Arduino Nano (or compatible).

  Each color column (or plane) MUST HAVE AN ASSOIATED SERIES RESISTOR !!

    NOTE! Keep the total max current for one row (3 RGB LEDs as white) within Arduino specs!
  
  The value used was chosen by experimentation, and is what my RGB LEDs seemed to be the most white with
  (with max but still a somewhat conservative intensity on R,G and B channels). That totalled about 8.5 mA for a fully lit RGB LED. Times 3 = 25.5 mA for one row, and below Arduino's max of 40 mA pr. pin 
  
  (Beware it's a 200 mA limit total all pins, regardless! According to Note 3 under Table 32-2 of the Atmega 328P datasheet) 
  
  Your RGB LED's may vary.
 


  The cube is really a 2D matrix of 3 columns by 9 rows.
  The 9 rows again are just positioned into a 3 by 3 pattern to make a cube (translated in software as a cube).
  One row is lit at a time, for a maximum of 1/9th intensity.

  Rows run along the X axis (width).
  Columns run along the Y and Z axis (a plane).


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
     Front   10  13  16    Back
             11  14  17
     ----------------------------  <-- Base (PCB / circuit board)


   Row numbering, the same right side view:

                 Top

              0   3   6
     Front    1   4   7    Back
              2   5   8
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

