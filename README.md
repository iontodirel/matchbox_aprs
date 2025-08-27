# Matchbox APRS

Miniaturized FSK modem and APRS platform in a compact and extensible 50×35mm "matchbox" form factor.

The modem is based on the Nino TNC design and firmware. NinoTNC firmware and circuit copyright (C) Nino Carrillo (https://ninotnc.com).

The APRS daughterboard adds standalone tracking and digipeating capabilities, with BLE/BT Classic access to the modem and Wi-Fi connectivity.

A 24-pin FFC expansion connector exposes modem mode selection, two serial ports (one to the TNC, one to USB), and status signals such as DCD and Packet Good.

Programming of the modem is supported through ICSP signals routed to the connector, allowing a Microchip PICkit 5 to directly flash the onboard dsPIC33 MCU.

Power is provided through a USB-C port via a low-noise LDO regulator with reverse-current protection. This design allows seamless switchover between USB and external 3.3 V supplies.

<img style="width:50%" alt="top" src="https://github.com/user-attachments/assets/63fa2b39-bfae-494b-a51c-1bb6c17f4b32" />

<img style="width:50%" alt="bottom" src="https://github.com/user-attachments/assets/f0faf837-3248-4b96-ba4d-68ec74348e0a" />

## Features

- Small form factor: 50x35mm board size
  - 0.8% larger than the Pico APRS v4 mainboard
  - 37% smaller than the Mobilinkd TNC4 mainboard
  - 63% smaller than the original Nino TNC mainboard
- Mainboard - modem
  - Power
    - Low noise regulator with reverse current protection
    - Supports 4–20 V input, up to 500 mA output
    - Can be bypassed (FCC2) or powered externally at 3.3 V
  - Interfaces
    - USB VBUS and CC1/CC2 available for optional USB-PD negotiation
      - Up to 3.2A from USB PD
      - Voltages higher than 5V can be negotiated and supplied to an off board regulator
    - Two UARTs (USB interface, TNC interface)
    - DCD and Packet Good status signals available on header
  - Standalone operation
    - Internal solder pads or resistor options for loopback serial operation and mode selection
    - Configurable digital modes via resistor population
  - Audio I/O
    - 3.5 mm TRRS jack (TXA/RXA/PTT), pinout selectable for Mobilinkd TNC4 or Digirig compatibility
  - Status
    - All of the 5 LEDs on the Nino TNC are routed to display modem status
  - Extensible interconnect for optional daughterboards
- Daughter board - Standalone Digipeater and tracker
  - Power management
    - Integrated 1.5 A Li-Po linear charger
    - Power-path management automatically switches between battery and USB
    - Buck-boost regulator provides stable 5V rail to the mainboard regardless of the battery voltage
    - Independent low-noise LDO for daughterboard circuitry
    - Two load switches allow MCU to power-gate the GNSS and mainboard and fully turn off both devices programatically
  - GNSS
    - Integrated GNSS receiver with onboard antenna
    - Retains warm-start capability via small rechargeable backup cell
  - Tracker
    - APRS packet types: position, Mic-E, compressed
    - Smart-beaconing algorithm
  - Digipeater
    - APRS routing: explicit, n-N
    - Supports all possible types of n-N addresses, ex: DIGIn-N
    - Preemtive digipeating: front, truncate, drop and mark
    - Viscous digipeating
  - Connectivity
    - USB serial
    - Wi-Fi
    - BL and BLE

## Connectors

### FCC1

| Pin | Label     | Description                                                                  |
|-----|-----------|------------------------------------------------------------------------------|
| 1   | USB VBUS  | USB bus supply                                                               |
| 2   | USB VBUS  | USB bus supply (duplicate pin for higher current or alternate routing)       |
| 3   | USB VBUS  | USB bus supply (duplicate pin for higher current or alternate routing)       |
| 4   | USB VBUS  | USB bus supply (duplicate pin for higher current or alternate routing)       |
| 5   | GND       | Ground                                                                       |
| 6   | GND       | Ground                                                                       |
| 7   | GND       | Ground                                                                       |
| 8   | GND       | Ground                                                                       |
| 9   | RX1       | UART1 receive input to the USB interface                                     |
| 10  | TX1       | UART1 transmit output from the USB interface                                 |
| 11  | RX2       | UART2 receive input to the TNC                                               |
| 12  | TX2       | UART2 transmit output from the TNC                                           |
| 13  | PKT       | Packet received signal                                                       |
| 14  | PGED1/M0  | ICSP data / Programming Data (PGED1) or Mode 0 selection                     |
| 15  | PGEC1/M1  | ICSP clock / Programming Clock (PGEC1) or Mode 1 selection                   |
| 16  | MCLR      | ICSP reset / Master Clear / Reset input for microcontroller                  |
| 17  | M2        | Mode 2 selection                                                             |
| 18  | M3        | Mode 3 selection                                                             |
| 19  | TEST TX   | Test transmit output                                                         |
| 20  | DCD       | Packet data clock detection                                                  |
| 21  | GND       | Ground                                                                       |
| 22  | +3.3V     | +3.3V supply from the LDO; can also be used to power the board               |
| 23  |           | Not connected / reserved                                                     |
| 24  |           | Not connected / reserved                                                     |

### FCC2 (optional)

FCC2 connector is optional if pins 3, 4, 5, 6, 11 and 12 are tied together using the on board resistor footprint

Pins 11 and 12 supplies 5V to the LDO. Higher voltage can also be supplied up to +20V

| Pin | Label     | Description                                                                  |
|-----|-----------|------------------------------------------------------------------------------|
| 1   | USB CC2   | USB PD configuration                                                         |
| 2   | USB CC1   | USB PD configuration                                                         |
| 3   | USB VBUS  | USB bus supply                                                               |
| 4   | USB VBUS  | USB bus supply (duplicate pin for higher current)                            |
| 5   | USB VBUS  | USB bus supply (duplicate pin for higher current)                            |
| 6   | USB VBUS  | USB bus supply (duplicate pin for higher current)                            |
| 7   | GND       | Ground                                                                       |
| 8   | GND       | Ground                                                                       |
| 9   | GND       | Ground                                                                       |
| 10  | GND       | Ground                                                                       |
| 11  | +5V       | LDO +5V supply, typically to be connected to USB VBUS                        |
| 12  | +5V       | LDO +5V supply, typically to be connected to USB VBUS                        |

## LDO

The on board LDO can run from as low as 4V for its maximum rated current of 500mA

Higher voltages can also be supplied to the LDO for up to +20V

The typical board consumption is about 50mA

Because the LDO has reverse current protection, the board can be powered externally from 3.3V, while simultanously powered by the USB-C port

## License

NinoTNC firmware and circuit copyright Nino Carrillo (https://ninotnc.com).

Gerber files copyright Ion Todirel. The gerber files can be used for non commercial purposes. The gerber files can be used by the Nino TNC project creators and contributors for any purpose as long as the following notice is reproduced:

```
matchbox APRS
Copyright (C) 2025 Ion Todirel
```

`libaprs`, `libaprsroute` and `libaprstrack` copyright Ion Todirel under MIT license. 

All the other assets in this repository are in the public domain.



