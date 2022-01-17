# AlphaMission/A.S.O. (Armored Scrum Object) arcade PCB PALs:
I decided to create this repository during the process of creating an FPGA Core for SNK's 1985 arcade game called Alpha Mission (which would have its sequel years later on the Neo Geo console) because dumps of existing PAL files I found they were incomplete and contained errors. Since I had a working original arcade board, I was able to capture data with a logic analyzer from the PAL ICs and contrasting these captures with the functions they were supposed to perform. Finally converting them to GAL devices and replacing them on the arcade board with the original PALs, I was able to test and verify that the modifications I made were correct.

## PAL16L8A_9F converted to GAL16V8 (20 pin device)
!["PAL16L8A 9F"](img/PAL16L8A_2-3.jpg?raw=true "PAL16L8A 9F")

Converted from PLD dump from Arian Mission to GAL16V8 format using handmade PAL file and PAL2GAL.EXE utility. Tested on AlphaMission pcb. The equations were simplified from unnecessary terms and contrasted using the evidence from the data capture of said PAL with a logic analyzer. Many thanks to @caiusarcade for the support.
(https://youtu.be/8plIKgkRWEY)

This JED file was burned onto a GAL16V8 device using a TL866II+ (XGecu Pro) programmer:

!["XGecu Pro"](img/XGECU_PRO.jpg?raw=true "XGecu Pro")

## PAL16L6_1A converted to GAL20V8 (24 pin device)
!["PAL16L6CNS 1A"](img/PAL16L6CNS_2-2.jpg?raw=true "PAL16L6CNS 1A")

The equations of this PAL were also corrected based on the captures of the real device with a logic analyzer and the source code of MAME, since this PAL is used to decode the address bus of the two main Z80 CPUs (CPUA, CPUB). Errors were found in two places: in the equations of the pin 21 responsible of background layer selection and in the equations of pin 16 related to the fixed layer selection. Conversion has been made to a GAL20V8 device and tested using a TL866II+ programmer with a Lattice GAL20V8B-25LP on the real PCB. Optionally lift the pin 18 of the GAL20V8 because is unused as output signal but because is linked in the PCB with the pin 20 ( of the same GAL20V8 device could interfere with the output of this pin. 

In my opinion, this bizarre connection was made due to the very structure of the PAL16L6 where there are only two product sums available for the output of pin 18 and four for the output of pin 20 (see the PAL16L6 datasheet). Since 4 product sums were needed for the signal selection of the background layer, said output of pin 20 was routed connecting it electrically with pin 18, whose output does not generate anything, and from here to the circuit where it activates the aforementioned background layer.

Was a bit tricky to program a GAL20V8B device with the JED file using the TL866II+ because this device often protested when it came to detecting the GAL20V8 pins, I don't know if they were defective, most of them were fake copies, or the recorder simply didn't work correctly with them, but finally I found a working one.

!["GAL20V8B-25LP"](img/GAL20V8B-25LP.jpg?raw=true "GAL20V8B-25LP")

File: **GAL20V8_AlphaMission_A1.jed**

## PAL16R6A_15B_FIXED converted to GAL16V8 (20 pin device)
!["PAL16R6A 15B"](img/PAL16R6A.jpg?raw=true "pal16r6a 15B")

Fixed PAL16R6 equations from Alpha Mission / A.S.O. dump and converted to GAL16V8 JED.  The JED file is available for recording on a GAL16V8 and the equations corrected with author's annotations. Recorded and tested using a TL866 II+ programmer with Xgpro v8.51. The fixed PAL can be downloaded also from the original site [PLD Archive](http://wiki.pldarchive.co.uk/index.php?title=Alpha_Mission).
These fixes are the result of analyzing signals from the PAL16R6 circuit on the original game PCB captured with a logic analyzer at 400MHz and compared to the simulation performed in Verilog.
The JED to equations conversions were made with MAME's JEDUTIL:
```
jedutil -view ArmoredScrumObject_pal16r6a_3.jed PAL16R6 > ArmoredScrumObject_pal16r6a_3_eqn.txt
```

```
jedutil -view "AlphaMission_ PAL16R6A_15B_FIXED.jed" GAL16V8 > "AlphaMission_ PAL16R6A_15B_FIXED_eqn.txt
```

The PAL16R6 to GAL16V8 conversions were made with Lattice PALTOGAL utility (from DOSBOX console).


## Testing on real PCB (Alpha Mission SNK ELECTRONICS A5002UP0x-01) with wrong PAL dump:
(https://youtu.be/Wj-3Nn4SDy8)

## Testing on real PCB (Alpha Mission SNK ELECTRONICS A5002UP0x-01) with fixed PAL dump:
(https://youtu.be/M8kRWPm-4Bg)

## Equations before fix:
```
Inputs:

2, 3, 4, 5, 6, 7, 8, 9, 12, 13, 14, 15, 16, 17, 18, 19

Outputs:

12 (Combinatorial, Output feedback output, Active low)
13 (Registered, Output feedback registered, Active low)
14 (Registered, Output feedback registered, Active low)
15 (Registered, Output feedback registered, Active low)
16 (Registered, Output feedback registered, Active low)
17 (Registered, Output feedback registered, Active low)
18 (Registered, Output feedback registered, Active low)
19 (Combinatorial, Output feedback output, Active low)

Equations:

/o12 = i2 & i4 & /i8 & /rf18 +
       /i8 & rf18 +
       i2 & i3 & i4
o12.oe = vcc

/rf13 :=
rf13.oe = OE

/rf14 := /i7 & rf18
rf14.oe = OE

/rf15 := /i7 & rf18
rf15.oe = OE

/rf16 := /i7 & /rf18 +
         /i7 & /rf18
rf16.oe = OE

/rf17 := /i4 +
         /i4
rf17.oe = OE

/rf18 := i2 & i4 +
         i2 & i4
rf18.oe = OE

/o19 = i7 +
       i7
o19.oe = vcc
```
## FIXED Equations (compare /rf15, /rf16, /o19 with the above):
```
Inputs:

2, 3, 4, 5, 6, 7, 8, 9, 12, 13, 14, 15, 16, 17, 18, 19

Outputs:

12 (Combinatorial, Output feedback output, Active low)
13 (Registered, Output feedback registered, Active low)
14 (Registered, Output feedback registered, Active low)
15 (Registered, Output feedback registered, Active low)
16 (Registered, Output feedback registered, Active low)
17 (Registered, Output feedback registered, Active low)
18 (Registered, Output feedback registered, Active low)
19 (Combinatorial, Output feedback output, Active low)

Equations:

/o12 = i2 & i4 & /i8 & /rf18 +
       /i8 & rf18 +
       i2 & i3 & i4
o12.oe = vcc

/rf13 := 
rf13.oe = OE

/rf14 := /i7 & rf18
rf14.oe = OE

/rf15 := i6 & /i7 & rf18
rf15.oe = OE

/rf16 := i6 & /i7 & /rf18
rf16.oe = OE

/rf17 := /i4
rf17.oe = OE

/rf18 := i2 & i4
rf18.oe = OE

/o19 = i7 +
       /rf18
o19.oe = vcc
```
