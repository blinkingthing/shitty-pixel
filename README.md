# shitty_pixel
This is mostly complete. I will add complete documentaion that includes puzzle spoilers after Defcon. 

## I2C ADDRESSES

7 bit Address | Device
--- | ---
0x50 | EEPROM
0x42 | LED Control

## EEPROM

I2C EEPROM Format adopted from ANDnXOR SAO Reference Design

0 | 1 | 2 | 3..n
--- | --- | --- | ---
DC Year | Maker ID | SAO Type ID | Data

* DC Year: Use 0x1B for DC27
* Maker ID: Unique identifier for SAO maker, I'm using **0x42**.
* SAO Type ID: Unique identifier assigned by the maker for the SAO
* Data: Arbitrary data parseable by anything recognizing DC, Maker, and SAO values

## LED CONTROL

The 3 LEDs are contorlled via I2C write commands. I was able to successfully send these commands via a Bus Pirate and the SAO v1.69bis Pi Developmemt Platform. No guarantee that any badge will be able to send these commands. 

#### Bus Pirate Example
Bus Pirate uses 8 bit address (7 bit address plus eighth R/W bit) - 0x42 = 0b1000010 , 0x84 = 0b1000010**0** 
```
I2C>[0x84 0x00 0x01] #turns LEDs on 
I2C>[0x84 0x00 0x00] #turns LEDs off
I2C>[0x84 0x00 0x03] #default Animation
```

#### Raspberry Pi SAO Dev Board Example
i2cset -y 1 <7-bit address> <8-bit register> <byte to be written>
```
i2cdetect -y 1 #detect whether or not there's anything on the I2C bus
i2cset -y 1 0x42 0x00 0x01 #LEDs on 
i2cset -y 1 0x42 0x00 0x00 #LEDs off
i2cset -y 1 0x42 0x00 0x03 #default Animation
```

7 bit Address | Register | Data | Command
--- | --- | --- | ---
0x42 | 0x00 | 0x00 | LEDs OFF
0x42 | 0x00 | 0x01 | LEDs ON
0x42 | 0x00 | 0x02 | Animation 1 : Fade
0x42 | 0x00 | 0x03 | Animation 2 : Rotate
0x42 | 0x00 | 0x04 | Animation 3 : Bounce
0x42 | 0x01 | 0x00 | Animation Speed (0x00-0xFF) : FAST
0x42 | 0x01 | 0xFF | Animation Speed (0x00-0xFF) : SLOW
0x42 | 0x02 | 0xFF | Red LED Max Brightness (0x00-0xFF): MAX BRIGHTNESS
0x42 | 0x02 | 0x7F | Red LED Max Brightness (0x00-0xFF): 50% BRIGHTNESS
0x42 | 0x03 | 0x7F | Green LED Max Brightness (0x00-0xFF): 50% BRIGHTNESS
0x42 | 0x04 | 0x00 | Blue LED Max Brightness (0x00-0xFF): 0% BRIGHTNESS
0x42 | 0x05 | 0x7F | Green LED Max Brightness (0x00-0xFF): 50% BRIGHTNESS
0x42 | 0x06 | 0x52 | Save Current State to EEPROM (persists through power cycle)
0x42 | 0x06 | 0x57 | Recall State from EEPROM

## SECRETS 
There's a bit of a crypto challenge included. Did you save the packaging the SAO came in? DM me @blinkingthing for hints if need be. 
