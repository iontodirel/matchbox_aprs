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
  - Serial interfaces can be routed together
  - Resistors can be used to configure the digital modes on board if wanted
- Small 2.5mm audio jack with TXA/RXA and PPT compatible with both Mobilinkd and Digirig interfaces
