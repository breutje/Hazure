# Hazure 「[外れ](https://defunct.nl/hazure/)」
The Hazure is a simple 6507 Single Board Computer.


## Status
⚠️ **Preliminary / In Active Development**


## Description
The [6507](https://en.wikipedia.org/wiki/MOS_Technology_6507) is a cost-reduced
[6502](https://en.wikipedia.org/wiki/MOS_Technology_6502) variant in a 28-pin DIP package,
famously used in the [Atari 2600](https://en.wikipedia.org/wiki/Atari_2600) and some floppy disk drives.
Because it lacks `A13–A15` address lines it can natively address only 8 kB of memory space without external banking.
As `IRQ`/`NMI` hardware interrupt pins are also missing, all I/O needs to be polled.
The software `BRK` however, is available.
The goal of Hazure is to build a minimal, functional retro SBC within these constraints.


The primary [ACIA](https://en.wikipedia.org/wiki/MOS_Technology_6551) handles standard terminal (tty) interaction,
while a dedicated **second ACIA** is provided specifically for downloading and uploading software and data.
This secondary channel avoids crowding the interactive console
and opens the door for protocols like DEC's [Radial Serial Protocol](http://bitsavers.informatik.uni-stuttgart.de/pdf/dec/dectape/tu58/EK-0TU58-UG-001_TU58_DECtape_II_Users_Guide_Oct78.pdf) (RSP) via a TU58 tape unit emulator.


## Features
- 6507 CPU
- Dual 6551 ACIA serial ports (TTL-level, 50 to 19200 bps or 38400 bps)
- 8 kB RAM (2 kB base and two software switchable banks of 3 kB)
- 8 kB ROM (four banks of 2 kB, DIP-switch selectable)
- 8 POST / Blinkenlight LEDs
- USB-C power
- 20-pin header for I/O expansion


## Memory map

| From  | To    | Size[^1] | Function                     |
| ----- | ----- | -------- | ---------------------------- |
| $0000 | $07FF | 2 kB     | RAM [^2]                     |
| $0800 | $13FF | 3 kB     | RAM (Bank RAM0 and RAM1)     |
| $1400 | $17FF | 1 kB     | I/O space                    |
| $1800 | $1FFF | 2 kB     | ROM (Bank ROM0 to ROM3) [^3] |


## Documentation
- [`HARDWARE.md`](./HARDWARE.md) — I/O map, clock generation, jumpers, expansion header
- [`SOFTWARE.md`](./SOFTWARE.md) — toolchain, monitor, ROM programming
- [`NOTES.md`](./NOTES.md) — design decisions and rationale 

[^1]: All sizes use tranditional binary kilobyte notation where 1 kB equals 1024 bytes.  
[^2]: This includes zero page and stack  
[^3]: Each bank includes the RESET and BRK vector at $1FFC  

