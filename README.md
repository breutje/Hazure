# Hazure 「外れ」
The Hazure is a simple 6507 Single Board Computer.


# Status
⚠️ **Preliminary / In Active Development**. See [`NOTES.md`](./NOTES.md) for architectural changes and rationale.


# Description
The [6507](https://en.wikipedia.org/wiki/MOS_Technology_6507) is a [6502](https://en.wikipedia.org/wiki/MOS_Technology_6502) variant in an cheaper 28-pin DIP package.
The 6507 was used in the [Atari 2600](https://en.wikipedia.org/wiki/Atari_2600) video game console as well as in some floppy disk drives.
It can only address 8kB of memory. It has no IRQ or NMI.
The challenge is thus, to create a usable retro SBC with these constraints.
It will need some ROM, RAM and at least a serial interface (tty).
To download or upload software, a second serial interface is provided.
Possibly using the Radial Serial Protocol [RSP](http://bitsavers.informatik.uni-stuttgart.de/pdf/dec/dectape/tu58/EK-0TU58-UG-001_TU58_DECtape_II_Users_Guide_Oct78.pdf) protocol of DEC's TU58 tape unit.
RSP can be emulated on a regular PC using open source software.

## Features
- 6507 CPU
- Two TTL level serial interfaces (50 .. 19200 bps or 38400 bps)
- 8kB RAM (2kB and two banks of 3kB, software select-able)
- 8kB ROM (four banks of 2kB, DIP-switch select-able)
- 8 POST/Blinkenlight LEDs
- USB-C power
- 20-pin header for some I/O expansion

# Memory map

| From  | To    | Function                    |
| ----- | ----- | --------------------------- |
| $0000 | $003F | RAM, including zero page    |
| $0040 | $07FF | RAM                         |
| $0800 | $0BFF | RAM RAM0/RAM1               |
| $0C00 | $0FFF | RAM RAM0/RAM1               |
| $1000 | $13FF | RAM RAM0/RAM1               |
| $1400 | $17FF | I/O                         |
| $1800 | $1BFF | ROM ROM0/ROM1/ROM2/ROM3     |
| $1C00 | $1FFF | ROM ROM0/ROM1/ROM2/ROM3     |

