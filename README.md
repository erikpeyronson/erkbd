# Erkbd

Erkbd is a 3D printed handwired 44 key keyboard with two encoders and two 1.3" 128x64 oled displays, based on the rp2040-zero development board.

It is inspired primarily by [crkbd](https://github.com/foostan/crkbd) and [void ergo s](https://github.com/victorlucachi/void_ergo)

The name is ment to honor the amazing corne which brought me into this hobby
combined with fact that my name is Erik. Hence ERiks KeyBoarD

## Things to be aware of before starting

If you decide to build this be aware that i a hobbyist and amateur, this is my
first 3D modeling project and there are a few issues.

**Case dimensions**

For aesthic reasons I wanted the caps to be as close as possible to the case
edge, it took some trial and error before i got it right and the clearance might
be small enough for a differently calibrated printer to cause problems. I do not
know.

Before you start soldering, assemble the case and plate with switches and
keycaps along the border and make sure the keycaps are not hitting the case and
let me know if it doesnt fit properly.

**Controller mount**

The edges of the controller mount was too low in the first revision and i ended
up having to use more hot glue than i would have liked to secure them. I have
raised them a little so the controllers should fit better and stay in place with
just friction but i have not tried this out since i have no more controllers.

**Switch holes**

The plate is 3mm thick but has pockets for the switches to
click in. They fit well enough for regular use but if you plan to change keycaps
often i recomend putting some hot glue to secure them or be very careful when
pulling the caps to not damage the matrix

**Bumpers** To keep the profile low and avoid the screws from hitting each other
in the middle of the spacers there are no pockets for the screws on the bottom
of the case. Without bumpons they will scratch your desk.

**Oled fit** There are different pcb versions for the OLED displays where the
pin headers are in slightly different places. I had both blue and white text
version the frame is modelled for the blue ones. but ended up using the white
ones

Its possible to use the white ones but the pin headers must be desoldered
and wire soldered so that they protrude as little as possible from the top of the pcb.

If you use the blue ones the headers should fit if as long as they are soldered
well and you cut them as close as the solder joint as possible on the top of the
pcb. These also have the ground and vcc pins reversed.

#### Solder and mount the OLEDS

Solder wire to the oled screens, make sure no note which pin is which.

**Entering bootloader**
There is no access to the reset button when the case is mounted. To enter bootloader you must use bootmagic by holding down key 0:0 when plugging the keyboard in.

## Support
I am a father and work full time hence i have limited time to provide support
but if you have any issues i will try my best to help.

## BOM
**plate**
- 46 pcs mx compatible switches and keycaps
- 2 pcs rp2040 zero controllers
- 2 pcs EC11 Rotary encoders
- 2 pcs DIP PJ320A trrs jacks
- 48 pcs1N4148 diodes
- 1 pcs 4.7K resistor (used for deciding handedness)
- Wire for the matrix. I tried using 26 AWG solid core wire for everything but
  found it to thick and ended up wiring the matrix separatly with solid core
  wire and connect to the controller with very thin stranded wire.

**Case**
- 18 pcs 5mm M2 brass female/female spacers (18 is a little overkill so some could be skipped)
- 36 pcs 5mm d2 brass screws (to match the above)
- Bumpons


**Oleds**
- 2 pcs  1.3" SH1107 oleds
- 8 pcs M3*12mm screws (for mounting the oleds)
- 8 pcs M3*bolts

## Firmware

The configuration and a default keymap that can be used for debugging can be found in my qmk fork  [here](https://github.com/erikpeyronson/qmk_firmware/tree/erkbd/keyboards/handwired/erikpeyronson/erkbd)

I do not use vial myself but i could try to build a vial firmware and upload here on request.

## Build steps

#### Printing

The keyboard consists of 3 parts. The switch plate, the case and the oled frame.
In addition to this you need to print 8 washers that sits between the oled pcb
and plate to make the components stay clear of the plate and align the top of
the frame with the bottom of the keycaps.

I printed using an Ender 3 V3 ke with the creality print slicer default settings
and Creality CR-PLA filament



#### Flash the controllers.

Before starting flash the controllers. The default firmware is a querty layout
with console enabled that logs the matrix events. Its only for debugging
purposes to use when building.

#### Handedness pin. LEFT side only
To identify which half is left and which half is right a 4.7K Pull up resistor
needs to be soldered between pin GP1 and 3.3V on the left half of the keyboard.

This is not the optimal position but i wrapped the legs in electrical tape and
bent it behind the usb port. Since both the oled and resistor needs to be
connected to 3v3 it is suitable to solder them both at the same time.

If you dont use the default firmware you can use an other pin if you like by
overriding the pin in config.h
```
#define SPLIT_HAND_PIN GP1
```

#### Solder and mount the OLEDS

Solder wire to the oled screens (see the note in the first section about oled
versions) make sure to note down which pin is which, you will not be able to see
the text on the pcs once the frame is mounted and the pins are different on
different versions

Feed the wire through the hole in the plate and and screw it in place using the
M3 screws. The order from the top is

- Oled frame
- Oled module
- Printed washers
- Switch plate

Solder the the wires to the controllers.

| Oled pin | Controller pin |
| :------- | :------------- |
| VDD      | 3.3v           |
| GND      | gnd            |
| SDA      | GP2            |
| SCL      | GP3            |

Remember the resistor on the right side

Plug it in and make sure the controller and oleds are working. The left side should print "left" and the right side "right" on the display

#### Mount m2 spacers.
Screw the m2 spacers into the plate before soldering. I learned the hard way that it is easy to accidentaly cover the holes with wires and diodes making case assembly impossible.

#### Wire the matrix
There are many guides for handwiring around, and i am not competent enough to provide anything better. I based my technique on [this](https://geekhack.org/index.php?topic=87689.0) but used the diode legs instead of wires to create the rows. If you have never done it before i encourage you to read multiple guides and select a technique you like.

#### Pins
The matrix pins are different on the right and left side. The default pins are as below

**Left side**

| Row | PIN |
| :-- | :-- |
| 0   | GP4 |
| 1   | GP5 |
| 2   | GP6 |
| 3   | GP7 |
|     |     |

**Right side**
| Column | PIN  |
| :----- | :--- |
| 0      | GP8  |
| 1      | GP9  |
| 2      | GP10 |
| 3      | GP11 |
| 4      | GP12 |
| 5      | GP13 |

if you want to use other pins you can override the following in config.h
```
#define MATRIX_ROW_PINS { GP4, GP5, GP6, GP7 }
#define MATRIX_COL_PINS { GP8, GP9, GP10, GP11, GP12, GP13}

#define MATRIX_ROW_PINS_RIGHT { GP28, GP27, GP26, GP15 }
#define MATRIX_COL_PINS_RIGHT { GP9, GP10, GP11, GP12, GP13, GP14}
```
And compile your own firmware

**Encoders**
When looked at from the bottom with the two pins pointing left and three pins to the right the order from the top is

| Pad    | Pin |
| :----- | :-- |
| A      | GP7 |
| GND    | GND |
| B      | GP8 |

The two on the other side are soldered to the matrix as a regular switch

these can be overriden with

```
#define ENCODERS_PAD_A { GP7 }
#define ENCODERS_PAD_B { GP8 }

#define ENCODERS_PAD_A_RIGHT { GP7 }
#define ENCODERS_PAD_B_RIGHT { GP8 }
```


**TRRS**

| ttrs | controller |
| :---- | :-- |
| 1 leg | GP0 |
| 1 leg | GND |
| 1 leg | 5v  |

Make sure you use the same legs on both halves. Double check with a multimeter before powering on.

This can be overriden by

```
#define SOFT_SERIAL_PIN GP0
```

#### Test, glue if needed, and assemble

Test everything before assembling. Make check the qmk console that all keys are properly registred and that you are able to flash using bootmagic by holding down key 0:0 when connecting the usb.

Add hot glue where needed, it "should" be possible to use no glue and have
everything fixed by friction but i recommend at least glueing the trs jack, and controller it makes mounting the case easier and will make it less fragile, when inserting the cable.

If it breaks you can always build another :)

Then insert the plate in the case and secure with the m2 screws.

Happy hacking.