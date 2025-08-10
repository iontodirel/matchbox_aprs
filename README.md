# Matchbox APRS

Miniaturized AFSK modem based on the Nino TNC design and firmware. NinoTNC firmware and circuit Copyright Nino Carrillo (https://ninotnc.com).

The modem offers all the same capabilities as the Nino TNC, in a matchbox 50x35mm form factor.

A 20 pin header routes the mode selections, two serial ports, one to connect to TNC, and one to USB. The DCD and "Packet Good" signals are also routed to the header.

The ICSP signals are routed to the header, where a Microchip PicKit 5 can be used to flash the dsPIC 33 MCU.

A low noise LDO supplies power from the USB-C power, with reverse current protection. This allows the board to be powered by a 3.3V external supply whether the USB connector is plugged in or not.

## Features

- Small form factor: 50x35mm
  - 0.8% larger compared to the Pico APRS v4 mainboard
  - 37% smaller compared to the Mobilinkd TNC4 mainboard
  - 63% smaller compared to the original Nino TNC mainboard
- Low noise regulator with reverse current protection
  - Allows the board to be powered by a 3.3V external supply
- DCD and Packet Good signals routed to the header
- USB VBUS routed to the header
- USB VBUS, USB CC1/CC2 routed to an optional header for optional PD negotiation
- Standalone operation
  - Serial interfaces can be connected together using solder pads or small resistors
  - Resistors can be used to configure the digital modes on board if wanted
- Small 3.5mm audio jack with TXA/RXA and PPT compatible with both Mobilinkd and Digirig interfaces
  - Jack type can be set using solder pads or populating small resistors

## Headers

### Header 1

| Pin | Label     | Description                                                                  |
|-----|-----------|------------------------------------------------------------------------------|
| 1   | +5V       | +5V supply                                                                   |
| 2   | +5V       | +5V supply (duplicate pin for higher current or alternate routing)           |
| 3   |           | Not connected / reserved                                                     |
| 4   | GND       | Ground                                                                       |
| 5   | GND       | Ground                                                                       |
| 6   | RX1       | UART1 receive input to the USB interface                                     |
| 7   | TX1       | UART1 transmit output from the USB interface                                 |
| 8   | RX2       | UART2 receive input to the TNC                                               |
| 9   | TX2       | UART2 transmit output from the TNC                                           |
| 10  | PKT       | Packet received signal                                                       |
| 11  | PGED1/M0  | ICSP data / Programming Data (PGED1) or Mode 0 selection                     |
| 12  | PGEC1/M1  | ICSP clock / Programming Clock (PGEC1) or Mode 1 selection                   |
| 13  | MCLR      | ICSP reset / Master Clear / Reset input for microcontroller                  |
| 14  | M2        | Mode 2 selection                                                             |
| 15  | M3        | Mode 3 selection                                                             |
| 16  | TEST TX   | Test transmit output                                                         |
| 17  | DCD       | Packet data clock detection                                                  |
| 18  |           | Not connected / reserved                                                     |
| 19  |           | Not connected / reserved                                                     |
| 20  | +3.3V     | +3.3V supply from the LDO; can also be used to power the board               |

### Header 2 (optional)

Header 2 is optional if pins 4, 5 and 6 are tied together using the on board resistor footprint

| Pin | Label     | Description                                                                  |
|-----|-----------|------------------------------------------------------------------------------|
| 1   | GND       | Ground                                                                       |
| 2   | USB CC2   | USB PD configuration                                                         |
| 3   | USB CC1   | USB PD configuration                                                         |
| 4   | USB VBUS  | USB bus supply                                                               |
| 5   | USB VBUS  | USB bus supply (duplicate pin for higher current, 2A max)                    |
| 6   | +5V       | +5V supply, typically to be connected to USB VBUS                            |
