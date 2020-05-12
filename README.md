# 333-RGB-LED-MiniCube

A small 3x3x3 RGB LED cube with 128 individual PWM levels for 81 LEDs (27 RGB) @ 52.3 Hz.
Basically just a test.

    DISCLAIMER

    This is provided on an as-is basis.
    It's only for a 3x3x3 cube (so "somewhat limited" resolution).
    The aim was (is?) an 8x8x8 cube.
    Some things are left unfinished, some unneccerasy bits included.

It's an indefinitely paused project...

    Modes (functions cycled with the one button):
    
    - Color cube test (RGB mapped to XYZ axis from black (as in off) to white)
    - Audio triggered animation cycling (not too good, a bit sensitive. More animations than with the button)
    - DNA spin (spinning twisted plane, periodically with and without blanking between frames)
    - RGB cycling all LEDs simultaneously (think one big RGB LED)
    - RGB slow cycling mood lamp thingy (kind of a plasma lamp, but much slower)
    - Blinking (every other LED)
    - Orbit (light orbiting center in complementary color)
    - 3-band equalizer / FFT Z-scroller (not so good either..)

    It's just some tests. I'm not too happy with all of them either.

Compiled using Arduino IDE (I last used version 1.8.10, but newer versions should work fine. Probably).
https://www.arduino.cc/

    NOTE
    You need to put all files in a folder of the same name as the main .ino file (without the .ino extension).
    Namely "333_RGB_LED_minicube_30_Release_1".
    Alternatively rename both (folder and main .ino file) to some other name.


Refer also to the included schematics (pdf).

    SCHEMATICS DESCRIPTION

Directly driven by and connected to an Arduino Nano (or compatible). There's also an electret mic preamp to make it react to sound in different ways. Using a 3.3V Zener to stabilize a voltage source for it (using Arduino's external ADC voltage reference) Atm I don't remember why I didn't use the Nano's built-in 3.3V source, but this is how I built it. So I included it.

    You don't have to use or include the mic / preamp thing if you don't want to,
    but the audio functions won't work then of course.

Each color column (or plane) MUST HAVE AN ASSOCIATED SERIES RESISTOR!

The value was experimentatally determined, and was what my RGB LEDs seemed to be the most white with (while still having a conservative current load). It totalled about 8.5 mA for a fully lit RGB LED. Times 3 = 25.5 mA for one row, and thus below Arduino's max of 40 mA pr. pin. In addition any one row pin is only on for 1/9th the time.
  
    NOTE! Keep the total max current for one row (3 RGB LEDs as white on one row pin) within 40 mA!

    Sidenote: There's a 200 mA limit total for all pins regardless!
   (Atmega 328P datasheet, Table 28.1: http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf#G1411831)
  
Your RGB LEDs may vary. Mine were common cathode, diffuse RGB LED's, not sure what particular brand it was.
 


The cube is really a 2D matrix of 3 columns by 9 rows.
The 9 rows again are just positioned into a 3 by 3 pattern to make a cube (translated in software as a cube).
One row is lit at a time, for a maximum of 1/9th intensity.

Rows run along the X axis (width).
Columns run along the Y and Z axis (a plane).


    Arduino digital output (pin) - to - RGB LED mapping:

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


Quick'n'dirty ASCII art RGB led cube building tip:

Drill 5mm holes (if your RGB LED's are 5 mm in diameter), in a 3x3 pattern in a piece of plank.
Space them about 15.25 mm apart (I think I used 1.5cm, but better to keep it in line with the standard 2.54 mm pitch)
Drill just enough so that a LED is fixed upside-down in the hole, to it's rim.

Make one column at a time.

Place one column of LED's, so that their pinouts is lined up perpendicular to the column's direction:
Assuming the pinout is BGCR (Blue - Green - common Cathode - Red)

    B          B          B
    G          G          G      ----> column direction
    C          C          C
    R          R          R


Then bend and solder all "color legs" together along this direction

    B----------B----------B----------
    G----------G----------G----------    ----> column direction
    C          C          C
    R----------R----------R----------

Do this for three columns total


    B----------B----------B----------
    G----------G----------G----------    ----> column 1
    C          C          C
    R----------R----------R----------


    B----------B----------B----------
    G----------G----------G----------    ----> column 2
    C          C          C
    R----------R----------R----------


    B----------B----------B----------
    G----------G----------G----------    ----> column 3
    C          C          C
    R----------R----------R----------

Do not snip off the last protruding legs!
Then bend the common Cathode pin above the others (no shorts!) and in the other direction.
(Ofc connect all Cathodes in the same row together).

      B----------B----------B----------
      G----------G----------G----------    ----> column 1
     /C         /C         /C
    | R--------|-R--------|-R----------
    |          |          | 
    |          |          | 
    | B--------|-B--------|-B----------
    | G--------|-G--------|-G----------    ----> column 2
    +-C        +-C        +-C
    | R--------|-R--------|-R----------
    |          |          |
    |          |          |
    | B--------|-B--------|-B----------
    | G--------|-G--------|-G----------    ----> column 3
    +-C        +-C        +-C
    | R--------|-R--------|-R----------
    |          |          |
    |          |          |
    |          |          |
    |          |          |
  
    Row N      Row N+1    Row N+2

         Row direction (downwards)

Build 3 of these 3 by 3 LED matrices.
Connect all matching columns together (so it'll be a plane)
Each column color leg must have their own resistor as mentioned above (depending on color and RGB LED specs).
All rows are held separate, and connected directly to the Arduino Nano's row pins (pin 9-17)
