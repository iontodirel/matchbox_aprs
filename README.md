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
    - Low noise linear regulator with reverse current protection
    - Supports 4–20V input, up to 500mA output
    - USB power can be bypassed (FCC2), supplied externally (via FCC2 from 4-20V), or the board can be powered externally by a 3.3V supply
  - Interfaces
    - USB VBUS and CC1/CC2 available for optional USB-PD negotiation
      - Up to 3.2A from USB PD
      - Voltages higher than 5V can be negotiated and supplied to an off board regulator via FCC2
    - Two UARTs (USB interface, TNC interface)
      - Can be connected together by onboard resistors in standalone operation
      - Can be router off the board via FCC2 
    - `DCD` and `Packet Good` status signals available on FCC2
  - Standalone operation
    - The mainboard can be configured to be used standalone without a daughter board 
    - Internal solder pads or resistors connect the USB serial interface to the TNC serial port
    - Placeholder resistors configure the TNC digital modes
  - Audio I/O
    - 3.5 mm TRRS jack (TXA/RXA/PTT), selectable for Mobilinkd TNC4 or Digirig compatibility
  - Status
    - All of the 5 LEDs on the Nino TNC are routed to display modem status
      - TX (red), DCD (yellow), RX (green), TX QUEUE (blue), CRC (red)
      - 0201 LEDs are used to minimize board space
  - Extensible interconnect for optional daughterboards (see FCC1 and FCC2 specification)
- Daughter board - Standalone Digipeater and tracker
  - Power management
    - Integrated 1.5 A Li-Po linear charger
    - Power-path management automatically switches between battery and USB
    - Buck-boost regulator provides stable 5V rail to the mainboard regardless of the battery voltage
    - Independent low-noise LDO for daughterboard power
    - Two load switches allow MCU to power-gate the GNSS and mainboard and fully turn off both devices programatically
    - Push button controller starts the system via a tactile switch with MCU soft power off
  - GNSS
    - Integrated GNSS receiver with onboard antenna
    - Retains warm-start capability via small rechargeable backup cell
    - GNSS receiver data can be used by the tracker, or passed through to the USB interface
  - Tracker
    - APRS packet types: position, Mic-E, compressed
    - Mic-E status
    - With or without timestamp (DHM/HMS formats)
    - Position ambiguity
    - Speed, course, altitude for both position packets and Mic-E packets
    - UTF-8 support
    - Smart-beaconing, Periodic or Manual beacon trigger
    - Flexible APRS encoding and tracker library powered by [libaprstrack](https://github.com/iontodirel/libaprstrack)
  - Digipeater
    - Powerful fully featured and fully extensible digipeater powered by [libaprsroute](https://github.com/iontodirel/libaprsroute)
    - APRS routing: explicit, n-N
    - Supports all types of valid n-N addresses, ex: DIGIn-N
    - Preemtive digipeating: front, truncate, drop and mark
    - Viscous digipeating
    - Digipeater implementation is completely standalone with no coupling to the MCU or other systems
  - KISS TNC over the USB or Bluetooth
    - GNSS data can also be routed throught the same interface
    - APRS packets can optionally be sent and received in plain text instead of AX.25
    - Simple text based command interface can be used to configure the modem, tracker, digipeater, and GNSS receiver 
  - RXA/TXA Calibration
    - TX calibration using Bessel null. The daughter board can instruct the modem to send a sweeping test signal in the TX chain.
    - RX calibration allows detecting RX chain signal clipping and help set the right sound volume on the radio.
  - Connectivity
    - USB serial
    - BT Classic and BLE

## Connectors

### FCC1

The 24 pin connector used is FFC2B28-24-G, it can be used with a flat flex cable 05-24-A-0030-A-4-06-4-T.

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

The 12 pin connector used is FFC2B28-12-G, it can be used with a flat flex cable 05-12-A-0050-A-4-06-4-T.

FCC2 connector is optional if pins 3, 4, 5, 6, 11 and 12 are tied together using the on board resistor footprint R7 marked "PWR"

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

## Mainboard LDO

The on board LDO on the can run from as low as 4V for its maximum rated current of 500mA

Higher voltages can also be supplied to the LDO for up to +20V

The typical board consumption is about 50mA

Because the LDO has reverse current protection, the board can be powered externally from 3.3V, while simultanously powered by the USB-C port

## License

Nino TNC firmware and circuit copyright Nino Carrillo (https://ninotnc.com).

Gerber files copyright Ion Todirel. The gerber files can be used for non commercial purposes. The gerber files can be used by the Nino TNC project creators and contributors for any purpose as long as the following notice is reproduced:

```
matchbox APRS
Copyright (C) 2025 Ion Todirel
```

[`libaprs`](https://github.com/iontodirel/libaprs), [`libaprsroute`](https://github.com/iontodirel/libaprsroute) and [`libaprstrack`](https://github.com/iontodirel/libaprstrack) copyright Ion Todirel under MIT license. 

All the other assets in this repository are in the public domain.



