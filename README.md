# Matchbox APRS

Miniaturized FSK modem and APRS platform.

The FSK modem is based on the Nino TNC design and firmware. NinoTNC firmware and circuit Copyright Nino Carrillo (https://ninotnc.com).

The modem offers all the same capabilities as the Nino TNC, in a matchbox 50x35mm form factor.

The APRS daughter board provides tracking and digipeating capability with BLE/BL access to the TNC and Wi-Fi connectivity. An optional uBlox receiver provides GPS.

A 20 pin header routes the mode selections, two serial ports, one to connect to the TNC, and one to USB. The DCD and "Packet Good" signals are also routed to the header.

The ICSP signals are routed to the header, where a Microchip PicKit 5 can be used to flash the dsPIC33 MCU.

A low noise LDO regulator supplies power from the USB-C connector, while providing reverse current protection. This allows the board to be powered by a 3.3V external supply whether the USB connector is plugged in or not.

<img style="width:50%" alt="top" src="https://github.com/user-attachments/assets/63fa2b39-bfae-494b-a51c-1bb6c17f4b32" />

<img style="width:50%" alt="bottom" src="https://github.com/user-attachments/assets/f0faf837-3248-4b96-ba4d-68ec74348e0a" />

## Features

- Small form factor: 50x35mm
  - 0.8% larger compared the Pico APRS v4 mainboard
  - 37% smaller than the Mobilinkd TNC4 mainboard
  - 63% smaller than the original Nino TNC mainboard
- Low noise regulator with reverse current protection
  - Allows the board to be powered by a 3.3V external supply
  - Regulator can be bypassed if desired via Header 2
  - LDO can operate from as high as 20V
- DCD and Packet Good signals are routed to the header
- USB VBUS routed to the header
- USB VBUS, USB CC1/CC2 routed to an optional header for optional PD negotiation
  - Up to 4A can be drawn from the USB interface with PD
  - Voltages higher than 5V can be negotiated and supplied to an off board regulator
- Standalone operation
  - Serial interfaces can be connected together using solder pads or small resistors
  - Resistors can be used to configure the digital modes on board if wanted
- Small 3.5mm audio jack with TXA/RXA and PPT compatible with either Mobilinkd TNC4 or Digirig
  - Jack type can be set using solder pads or populating small resistors
- Extensible interconnect system can connect to the TNC and provide additional capabilities via daughter boards
- All of the 5 LEDs on the Nino TNC are routed to display modem status

## Headers

### Header 1

| Pin | Label     | Description                                                                  |
|-----|-----------|------------------------------------------------------------------------------|
| 1   | USB VBUS  | USB bus supply                                                               |
| 2   | USB VBUS  | USB bus supply (duplicate pin for higher current or alternate routing)       |
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

Pin 6 supplies 5V to the LDO. Higher voltage can also be supplied up to +20V

| Pin | Label     | Description                                                                  |
|-----|-----------|------------------------------------------------------------------------------|
| 1   | GND       | Ground                                                                       |
| 2   | USB CC2   | USB PD configuration                                                         |
| 3   | USB CC1   | USB PD configuration                                                         |
| 4   | USB VBUS  | USB bus supply                                                               |
| 5   | USB VBUS  | USB bus supply (duplicate pin for higher current, 2A max)                    |
| 6   | +5V       | LDO +5V supply, typically to be connected to USB VBUS                        |

## LDO

The on board LDO can run from as low as 4.3V for its maximum rated current of 500mA

Higher voltages can also be supplied to the LDO for up to +20V

The typical board consumption is about 50mA

Because the LDO has reverse current protection, the board can be powered externally from 3.3V, while simultanously powered by the USB-C port
