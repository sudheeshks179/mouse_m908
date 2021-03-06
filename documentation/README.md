# Documentation

## Source documentation
Doxygen is used in places for source code documentation. To build the documentation install doxygen and run
```
doxygen Doxyfile
```
A directory named doxygen will be created, containing html and latex files.

## USB Protocol description
This directory contains captured usb data (when sending the default settings). For further details look at the write functions in the source code.

default_m908.txt and default-annotated_m908.pdf contain the extracted usb data when sending the default settings. This was obtained by exporting packet dissections as plain text from wireshark (select only bytes) and running ``grep "00[4-9]0"`` on the exported files.


### Button mapping
The function of each button is described by 4 bytes. In case of the fire button all 4 bytes are used, in all other cases the last byte is 0x00. Look at ``set_key_mapping()`` (include/setters.cpp) and include/data.cpp for the full meaning of these bytes.

A few are listed below:
- Keyboard key:
	0. = 0x90 if without modifiers, = 0x8f if with modifiers
	1. modifier value (sum of individual modifer values)
	2. keycode
	3. = 0x00
- Media keys:
	0. = 0x8e
	1. = 0x01
	2. keycode
	3. = 0x00
- Fire button:
	0. = 0x99
	1. keycode
	2. repeats
	3. delay
- Macro:
	0. = 0x91
	1. macro number (0x0-0xe)
	2. number of repeats
	3. 0x00
- Macro (repeat until button is pressed again):
	0. = 0x91
	1. macro number (0x0-0xe) + 0x3f
	2. 0xff
	3. 0xff
- Macro (repeat while button is pressed):
	0. = 0x91
	1. macro number (0x0-0xe) + 0x7f
	2. 0xff
	3. 0xff
- Snipe (Changes DPI while pressed)
	0. = 0x9a
	1. 1
	2. DPI 200-1100 (100 increment): 4 6 9 b d f 12 14 16 18
	3. = 2.
- No function (none)
	all bytes = 0x00

### Reading settings from the mouse
The settings are read in 3 parts:
1. led settings
2. macros
3. button mappping

### LED modes
The LED mode is specified by two bytes.

byte1 % 8:
	- 0: off
	- 1: decode byte2
	- 2: wave (multicolor)
	- 3: reactive_button (static color, new color on button press, not in the official software)
	- 4: random (random flashing with random colors, scrollwheel and logo separately, not in the official software)
	- 5: wave (same as 2)
	- 6: alternating (flashing of scrollwheel and logo separately, multicolor)
	- 7: reactive

byte2 (only relevant when byte1 == 1):
	- 0: breathing (single color)
	- 1: breathing_rainbow (not in the official software)
	- 2: static
	- 8: rainbow
	- 16: flashing
