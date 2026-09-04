# Hardware


## Memory map

The memory map in 1kB blocks:

| From  | To    | Function                    |
| ----- | ----- | --------------------------- |
| $0000 | $003F | RAM, including zero page    |
| $0040 | $07FF | RAM                         |
| $0800 | $0BFF | RAM, bank switched          |
| $0C00 | $0FFF | RAM, bank switched          |
| $1000 | $13FF | RAM, bank switched          |
| $1400 | $17FF | I/O                         |
| $1800 | $1BFF | ROM                         |
| $1C00 | $1FFF | ROM, including reset vector |

The ROM's are selected using two DIP switches and have 4 banks of 2 kB each.

## Interal I/O
The 1 kB I/O block is split in internal I/O and external I/O.
The internal block is 512 bytes ($1400 to $15FF).

Possible external peripherals are: EF9345 video, keyboard and 8-bit CF (IDE) storage.
The 1kB I/O block is used for the 6551 ACIA's and a single 8-bit 74HCT574 POST/Blinkenlights register.

| Address | Function                 |
| ------- | ------------------------ |
| $1400   | ACIA #1 Data Register    |
| $1401   | ACIA #1 Status Register  |
| $1402   | ACIA #1 Command Register |
| $1403   | ACIA #1 Control Register |
| $1404   | ACIA #2 Data Register    |
| $1405   | ACIA #2 Status Register  |
| $1406   | ACIA #2 Command Register |
| $1407   | ACIA #2 Control Register |
| $1408   | POST / Blinkenlights     |
| $1409   | BANK switch on bit 7     |

## External I/O
The external I/O block is the second half of the 1 kB I/O space ($1600 to $17FF).
This is decoced as `EXT_IO`.
The external peripherals are expected to decode the right address further using `A0` to `A9`

## Projected I/O

| From   | To     | Function                  |
| ------ | ------ | ------------------------- |
| $1600  | $1601  | Reserved for keyboard     |
| $1602  | $1604  | Reserved for EF9345 video |
| $1604  | $1614  | Reserved for CF           |

## Peripheral bus
The peripheral bus is exposed on the peripheral board via a 2x12 100 mil right-angle male header,
which mates with a female socket on the CPU board.

| Signal | PIN | PIN | Signal |
| ---: | :---: | :---: | :--- |
| +5V | 1 | 2 | GND |
| D0 | 3 | 4 | D1 |
| D2 | 5 | 6 | D3 |
| D4 | 7 | 8 | D5 |
| D6 | 9 | 10 | D7 |
| A0 | 11 | 12 | A1 |
| A2 | 13 | 14 | A3 |
| A4 | 15 | 16 | A5 |
| A6 | 17 | 18 | A7 |
| A8 | 19 | 20 | /EXT_IO |
| Φ2 | 21 | 22 | R/W |
| /RESET | 23 | 24 | SYS_CLK |

