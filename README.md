# OneNee_Ch32v003
PsNee + OneChip port to the ch32v003 for PAL PM41 boards.

Port of PsNee v7/v8 with OneChip BIOS patching for the WCH ch32v003 MCU. Full stealth and BIOS patching on PAL PM41-v1 and PM41-v2 boards! 
Virtually any noise or degradation to the laser RF signal level because the code injects the SCEX string only when needed. 
This code is ONLY compatible with PM-41v1 and PM-41v2 PAL motherboards.

I'm just a hobbyist programmer, so please forgive any coding mistakes, syntax errors, or other issues. The code is heavily based on PsNee v7/v8 by kalymos, 
but this time I've chosed to not follow the Arduino portability philosophy. 
Code is written using ch32fun libs avoiding HAL when possible. The result are less portability but a faster, lighter, efficient and overall way better code!

Why I haven't ported "postal" PsNee v8? Simply because JAP bios patching is bugged (with some BIOS menu crashes) and until a fix came out i'm not interested in a porting...

For the 8-pin ch32v003j4m6 i'm using the pin multiplexing feature, so you can use "pin-8" (as input only!) for the console point Bios_a18 
and, if you need reprogramm the chip, you can do without any bricking risk!

**Warning:**
- This code is only compatible with Psone Scph-102 with PAL bios. For the other models, you can use my [PsNee_ch32v003](https://github.com/carmax91/PsNee-CH32V003) port.

## Features

- Full Stealth!
- Virtually any noise or degradation on the laser RF signal level unlike the old oldcrow\mayumi\multimode mod.
- PAL Bios patching feature for USA and JAP games.
- No atmega328p+16mhz cristal or other arduino uno like "big" board, simply a 8-pin ch32v003j4m6 chip to solder that can be easly fitted in your console!
- Bin files should be compatible even on other ch32v003 package (SOP-16, TSSOP-20 and QFN-20) and valutation board provided that you remove the external osc!

## Supported Playstations

 - PSone SCPH-102 ONLY!.
 - For other models, you can use my [PsNee_ch32v003](https://github.com/carmax91/PsNee-CH32V003) port.

## Prerequisites
- [WCH-LinkE](https://github.com/carmax91/OneNee_Ch32v003/blob/main/Imgs/WCH-LinkEPrg.jpg) programmer (pay attention to the E).
- WCH LinkUtility software.

## HowTo
- Download this repository.
- Install the WCH LinkUtility software and the releative drivers (all the the needed software/files are zipped in the "tool" directory).
- Connect the LinkE programmer and check if is in RISCV mode (Blue LED should be always off when idle).
- If not, disconect and reconnect the programmer holding down the "Mode-S" button.
- Open LinkUtility and check if the tool sees the programmer.
- Set the core (RISC-V) and the series (CH32V003).
- Click on the folder icon (or press ALT+F1)
- Select the compiled OneNee_Fun[PAL].BIN file (you can find the bin file in the [BIN](https://github.com/carmax91/OneNee_Ch32v003/tree/main/BIN) folder).
- Connect the chip to the programmer.
- Program the chip via Target -> Program (or press F10).
- ![done](https://github.com/carmax91/OneNee_Ch32v003/blob/main/Imgs/Linw.jpg)
- Now you have your OneNee on WCH CH32V003 chip!
- For the installation, follow the wiring diagrams in the [Install](https://github.com/carmax91/OneNee_Ch32v003/tree/main/Install) folder based on your console motherboard.

## Some info about the source code
I've ported the code to the ch32fun platform wich is lighter faster and ages better than the official WCH bugged arduino platform!
All in bare-metal and the difference is huge!!!!
Even because we don't have to carry anymore all the bloatware (super bugged) HAL of arduino libs! So now the code is way faster and efficient!

## Why this modc. is better and different compared to the older oldcrow\mayumi\multimode\onchip\stealth2.x? (extracted part from PsNeev7 readme)

The original code doesn't have a mechanism to turn the injections off. It bases everything on a timer. After power on, it will start sending injections for some time, then turns off. 
It also doesn't know when it's required to turn on again (except for after a reset), so it gets detected by anti-mod games.
The mechanism to know when to inject and when to turn it off.

This is the 2 wires for SUBQ / SQCK. The PSX transmits the current subchannel Q data on this bus. It tells the console where on the disc the read head is. We know that the protection symbols only exist on the earliest sectors, and that anti-mod games exploit this by looking for the symbols elsewhere on the disk. If they get those symbols, a modchip must be generating them!

So with that information, my code knows when the PSX wants to see the unlock symbols, and when it's "fake" / anti-mod. The chip is continously looking at that subcode bus, so you don't need the reset wire or any other timing hints that other modchips use. That makes it compatible and fully functional with all revisions of the PSX, not just the later ones. Also with this method, the chip knows more about the current CD. This allows it to not send unlock symbols for a music CD, which means the BIOS starts right into the CD player, instead of after a long delay with other modchips.

This has some drawbacks, though:

- It's more logic / code. More things to go wrong. The testing done so far suggests it's working fine though.
- It's not a good example anymore to demonstrate PSX security, and how modchips work in general.


## Thanks to:
ramapcsx2, kalymos, SpenceKonde, oldcrow, mayumi, arduino community, ch32fun community and lots of people that can't remeber now.
