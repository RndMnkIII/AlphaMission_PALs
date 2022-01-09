# AlphaMission/A.S.O. arcade PCB PALs:
## PAL16L8A_9F converted to GAL16V8 (20 pin device)
Converted from PLD dump from Arian Mission to GAL16V8 format using handmade PAL file and PAL2GAL.EXE utility. Tested on AlphaMission pcb. The equations were simplified from unnecessary terms and contrasted using the evidence from the data capture of said PAL with a logic analyzer. Many thanks to @caiusarcade for the support.
(https://youtu.be/8plIKgkRWEY)

## PAL16L6_1A converted to GAL20V8 (24 pin device)
The equations of this PAL were also corrected based on the captures of the real device with a logic analyzer and the source code of MAME, since this PAL is used to decode the address bus of the two main Z80 CPUs (CPUA, CPUB). Conversion has been made to a GAL20V8 device but it has not been possible to record in a real one since I have not managed to obtain one that does not give problems when recording it with the TL86+, it may because all the ones I have bought so far from China are false imitations of a GAL20V8B.
If someone can advise me on where to obtain a GAL20V8B-25LP from a reliable source and that the device works correctly or can offer me one to test the validity of the JED file, it will be welcome.
File: GAL20V8_AlphaMission_A1.jed

## PAL16R6A_15B_FIXED converted to GAL16V8 (20 pin device)
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
