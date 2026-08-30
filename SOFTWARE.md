# Software
Four banks of 2 kB ROM can be selected with 2 DIP switches.

| SW1 | SW0 | Bank | ROM         |
| --- | --- | ---- | ----------- |
|  0  |  0  |   0  | Monitor     |
|  0  |  1  |   1  | Forth       |
|  1  |  0  |   2  | Lisp        |
|  1  |  1  |   3  | Application |

After applying the swiches, Hazure has to be rebooted.
Either by applying power or using the reset switch.
What each 2 kB bank should contain is not determined yet.

## Monitor
I assume only bank 0 should have a 6502 monitor.
The most obvious candidate was Wozmon: the original monitor for the Apple 1, written by Steve Wozniak.
With less than 256 bytes, this fits easily in a 2 kB bank.
The original version assumed a matrix keyboard was connected to a PIA (6820, 6821, 6520, or 6521 chip).
We therefore need a variant that is modified to use the ACIA serial port.
We used Ben Eater's version modified for the 6551 ACIA.
Ben Eater was using the WDC 65C51.
This chip has the infamous Transmit Data Register Empty (TDRE) bug.
The TDRE bit will not reliable be set if the transmit buffer is empty.
This will lead to infinite loops.
The usual fix is in the [code]( https://gist.github.com/beneater/8136c8b7f2fd95ccdd4562a498758217) by Ben Eater: add a delay loop.
This is not necessary for Hazure, as it uses the original 6551 or 6551A NMOS chips.
The timing is checked for 19200 bps, but there is a 38400 option that may require a different delay.
Also, Hazure has the option for a 1.8432 MHz CPU clock.
I modified Ben Eater's code to use the TDRE.
Do not attempt to use this on a WDC 65C51 ACIA.


## Forth
Here, [milliForth](https://github.com/fuzzballcat/milliForth) is the most obvious candidate.
It will fit in 512 bytes, leaving 1.5 kB spare for extra words and a RDS implementation.


## LISP
[sectorLisp](https://github.com/jart/sectorlisp) is a possibility,
but I'm leaning towards a custom implementation of QuectoLisp, called HazureLISP for the occasion.
This allows me to utilise the 2 kB ROM bank to the fullest.


## Application
An interesting application as demo would be Joseph Weizenbaum's [ELIZA](https://en.wikipedia.org/wiki/ELIZA).
ELIZA was originally implemented in [MAD-SLIP](https://en.wikipedia.org/wiki/SLIP_(programming_language))
but various implementations, as well as the original source code can be used as starting point for an Hazure Eliza implementation.
[Anthony Hay](https://github.com/anthay/ELIZA/tree/master)'s C++ version can be used to get a reasonable implementation.

## Common
All 2 kB ROM banks start at `$1800` and require the `MNI`, `RESET` and `IRQ` vectors at `$1FFA`.
The `NMI` vector cannot be used.
The `RESET` vector should point to the entry point of the software.
The `IRQ` vestor is still useful as the `BRK` instruction also uses that vector.
Common to all banks:

- vector table at `$1FFA`
- initialisation of at least the terminal ACIA at `$1400`
- clearing the POST LEDs and lighting a meaningful pattern
- .org is alwaya at `$1800`


# Disaster recovery
Experimenting is highly encouraged.
But it is entirey possible you will "brick" the Hazure at some point.
In order to speed-up recovery, There is an 8 kB [diagnostics.bin](./diagnostics/diagnostics.bin) image in the `diagnostics` folder on github.
This image contains several tests.
The best place to keep it handy is in a 2764 UV EPROM before anything goes wrong.
Don't forget to add a sticker on the UV window.
Remove your current ROM and place the 2764.
Check that all pins seat and that pin 1 is in the right spot.
Plug in the loopback connectors or use a female-female harwin wire to connect TX with RX.
Set the ROM jumpers for EPROM and power up.
The detailed instruction is on the [hardware](./HARDWARE.md) page
The POST LEDs should remain off for about a second.
The following table will give an indication what the lastest checkpoint has passed.
Further detailed information can be found on the [diagnostics](./diagnostics/diagnostics.md) page.


|7|6|5|4|3|2|1|0|Code| Checkpoint  (passed)        |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:---:|:---|
|⚪|⚪|⚪|⚪|⚪|⚪|⚪|⚪| $00 | CPU startup |
|⚪|⚪|⚪|⚪|⚪|⚪|⚪|🔴| $01 | Base RAM |
|⚪|⚪|⚪|⚪|⚪|⚪|🔴|⚪| $02 | Bank 0 RAM |
|⚪|⚪|⚪|⚪|⚪|⚪|🔴|🔴| $03 | Bank switch |
|⚪|⚪|⚪|⚪|⚪|🔴|⚪|⚪| $04 | RAM 1 RAM |
|⚪|⚪|⚪|⚪|⚪|🔴|⚪|🔴| $05 | ACIA0 present |
|⚪|⚪|⚪|⚪|⚪|🔴|🔴|⚪| $06 | ACIA0 initialized |
|⚪|⚪|⚪|⚪|⚪|🔴|🔴|🔴| $07 | ACIA0 TX/RX loopback |
|⚪|⚪|⚪|⚪|🔴|⚪|⚪|⚪| $08 | ACIA1 present |
|⚪|⚪|⚪|⚪|🔴|⚪|⚪|🔴| $09 | ACIA1 initialized |
|⚪|⚪|⚪|⚪|🔴|⚪|🔴|⚪| $0A | ACIA1 TX/RX loopback |
|⚪|⚪|⚪|⚪|🔴|⚪|🔴|🔴| $0B | BRK test passed |


The diagnostics program is also available as a 2 kB image to add to a bank in your own ROM.