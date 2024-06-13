# Component_LCR_tester

1. HARDWARE 
    - Hardware used is from https://github.com/Mikrocontroller-net/transistortester/tree/master/Hardware/PCB_mega328_ST7735 
	- PCB was scratched (=engraved) using my sister's Cricut and a blank, single-sided piece of cupperclad board. 
    - After populating the PCB with all of its components, the microcontroller needs to be configured. 
    - The .HEX and .EEP files included in the folder 
		    https://github.com/Mikrocontroller-net/transistortester/tree/master/Software/trunk/ST7735 
	    can be used for initial testing, but these need to be adjusted. 
	    The HEX file is an Intel HEX format file to be loaded into the flash memory of the ATMega328. 
	    The EEP file is an Intel HEX format file to be loaded into the EEPROM memory of the ATMega328. 
	    The fuse settings should be: Low byte: F7; High byte: D9; Extended byte: FC. 
	    You can use Arduino's AVRDUDE or XGECU T48 to load both files into and set the fuses of the ATMega328. 
    - The parts list calls for two generic NPN and one generic PNP transistors. 
    - In the 3D-view, they are listed as 2N3904 and 2N3906 resp. and are oriented with the flat sides to the top of PCB. 
    - However, the pin-out of the BC547 and BC557 transistors which I used is different. 
    - As a result, initial testing failed to light the power LED properly and pushing the rotary switch didn't register. 
    - Swapping the orientation of all transistors and ensuring the C/B/E were properly connected, fixed this issue. 

2. SOFTWARE 
    - Use "git clone https://github.com/jonsag/avrComponentTester.git" to download all source files 
    - Note: no .HEX or .EEP file is included, but the two files above can be used for testing. 
    - To produce the two new .hex and .eep files, the MAKEFILE must be used to produce 
	        - mega328_color_kit.hex 
	        - mega328_color_kit.eep 
    - But since the project is designed to be compiled on Linux, we need some tools to run it under Windows: 
	    - Install and configure MSYS2 using instructions from msys2.org 
	    - Install and configure MinGW64 using instructions from mingw-w64.org 
	    - These two are needed to have a MAKE utility and allow using the project's MAKEFILE 
	    - The Arduino's avr-gcc compiler is needed to compile and link the C and ASM files 
    - After running the MAKEFILE for the first time, two new .hex and .eep files were created and downloaded. 
    - The text was appearing 90 degrees rotated counter-clockwise. 
    - So in the MAKEFILE, the value for 
	    	CFLAGS += -DLCD_ST7565_H_FLIP=1
	    was changed to 0. 
    - Re-running the MAKEFILE and re-downloading showed all was OK now. 
    - A small border of 3 rows of random pixels is still showing. 
    - Changing the value of 
	    	CFLAGS += -DLCD_ST7565_H_OFFSET=0 
	    to 2 or 4 might fix it. Yet to be tested. 

Similar projects can be found at: 
    - Arduino-based design, described in the "Razzies" magazine April 2019 (https://www.pi4raz.nl/razzies/razzies201904.pdf)
    - https://www.electronics-diy.com/esr-meter.php
    - https://www.electronics-lab.com/component-tester-2/
    - https://www.youtube.com/watch?v=Z705XVfkb5w&t=284s
    - https://www.instructables.com/Component-Tester-Test-Almost-Anything-/
    - https://github.com/kr4fty/Ardutester
    - https://hackaday.io/project/175338-component-tester-uno-shield
    - https://trybotics.com/project/ardutester-v1-13-the-arduino-uno-transistor-tester-dbafb4
    - http://digilander.libero.it/nick47/ard07.htm
    Several of these are based on the original mikrocontroller.net design. 

To do: build an enclosure and prettify the unit. 
