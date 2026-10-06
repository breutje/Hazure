# Hardware


## Memory map

The memory map in 1kB blocks:

| From  | To    | Function                    |
| ----- | ----- | --------------------------- |
| $0000 | $03FF | RAM, including zero page    |
| $0400 | $07FF | RAM                         |
| $0800 | $0BFF | RAM                         |
| $0C00 | $0FFF | RAM                         |
| $1000 | $13FF | RAM0, RAM1 (`/RAM_BANK`)    |
| $1400 | $17FF | RAM0, RAM1 (`/RAM_BANK`)    |
| $1800 | $1BFF | ROM                         |
| $1C00 | $1FFF | ROM, I/O and reset vectors  |

The ROM's are selected using two DIP switches and have 4 banks of 2 kB each.

## I/O
I/O is a block of 16 bytes from $1FE0 to $1FEF.
This means it is inside the ROM address space.
Independent of the ROM switches, $1FE0 to $1FEF is I/O space.

| Address | Function                        |
| ------- | ------------------------------- |
| $1FE0   | ACIA #1 Data Register           |
| $1FE1   | ACIA #1 Status Register         |
| $1FE2   | ACIA #1 Command Register        |
| $1FE3   | ACIA #1 Control Register        |
| $1FE4   | ACIA #2 Data Register           |
| $1FE5   | ACIA #2 Status Register         |
| $1FE6   | ACIA #2 Command Register        |
| $1FE7   | ACIA #2 Control Register        |
| $1FE8   | POST / Blinkenlights            |
| $1FE9   | `RAM_BANK` switch on bit 7      |
| $1FEA   | PCF8584 Data register           |
| $1FEB   | PCF8584 Control/Status register |

## ROM Socket
The ROM socket can take 27C64 (or 2764) EPROMs, 28C64 EEPROMs or FM1608 F-RAMs.
There is one jumper: write protect / enable.

| Pin | 27C64     | 28C64         | FM1608 | Remark                |
| :-: | --------- | ------------- | ------ | --------------------- |
|  1  | VPP /VCC  | RDY-/BUSY /NC | NC     | 10k pull-up to Vcc    |
|  2  | A12       | A12           | A12    | pull-down ROM1 switch |
| 23  | A11       | A11           | A11    | pull-down ROM0 switch |
| 26  | NC        | NC            | NC     | leave unconnected     |
| 27  | /PGM /VCC | /WE           | /WE    | jumper: Vcc or /WE    |

## Power & USB
Power is provided by an USB-C connector and two 5.1 kΩ resistors.
The connector will be an USB4085-GF-A hybrid USB-C connector.
There may be a possibility to use a MCP2221 to connect the USB to ACIA #1


## I²C
I²C may be provided though a PCF8584 I²C-bus controller.
This does require an additional 12MHz oscillator.
Investigate: possbily can be divided to provide 2MHz or 1MHz CPU clock as well.
