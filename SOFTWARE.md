# Software
Four banks of 2 kB ROM can be selected with 2 dip switches.

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
The most obvious candidate is Wozmon.
The original monitor for the Apple 1, written by Steve Wozniak.
With 256 bytes, this should fit easily in a 2 kB bank.
It did assume the matrix keyboard was connected to a PIA (6820, 6821, 6520, or 6521 chip).
We therefore need a variant that is modified to use the ACIA serial port.
We can use Ben Eater's version modified for the 6551 ACIA.


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
All 2 kB ROM banks start at $1800 and require the RESET vector at $1FFC and BRK vector at $1FFE.
The initialization of the ACIAs may be common to all banks.
Maybe a few common routines can be provided too.

