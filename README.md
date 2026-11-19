# OpenRX Lite-UFL: Antenna Diversity Fork (RF Switching)

Fork of the open-source [OpenRX Lite-UFL](https://github.com/OpenDrone-hw/OpenRX-Lite-UFL) 2.4 GHz ExpressLRS receiver, modified to add **antenna diversity** using an RF switch and two U.FL connectors. This is a learning project and is **work in progress**. The hardware is not yet designed, built, or tested.

[OpenRX Lite-UFL front](https://github.com/KardalHaist/OpenRX-Lite-UFL_RF_Switching/blob/main/images/front.png) [OpenRX Lite-UFL back](https://github.com/KardalHaist/OpenRX-Lite-UFL_RF_Switching/blob/main/images/back.png)

[Status](https://github.com/OpenDrone-hw/.github/blob/main/CONTRIBUTING.md#the-life-of-a-project) [Shop](https://opendrone.be/products/openrx) [Discord](https://discord.com/channels/1494019459822653512/1494758332903456969) [Video](https://www.youtube.com/watch?v=ssmQkRkXE84) [OSHWA](https://certification.oshwa.org/be000031.html)

## About the original

OpenRX Lite-UFL is an open-source 2.4 GHz ExpressLRS receiver with telemetry, for any radio-controlled build: cars, boats, robots and aircraft. It combines an ESP32-C3 and SX1281 with a U.FL antenna connector in a 10.0 x 11.5 mm board, and passes the received channels over serial (CRSF) on its RX and TX pads to a controller such as a flight controller or a vehicle or robot controller. All credit for the base design goes to the OpenDrone-hw team.

<p> <img src="images/front.png" width="400" alt="OpenRX Lite-UFL front" /> </p>
## What this fork changes

The original has one radio path: SX1281 → bandpass filter → one U.FL connector. This fork adds a second antenna:

- **One radio, one RF switch, two U.FL connectors.** An SPDT (single-pole, double-throw) RF switch connects the radio's RF port to either antenna, controlled by one ESP32-C3 GPIO.
- **Antenna diversity, not true diversity.** The receiver listens on one antenna at a time and switches when link quality drops. True diversity needs two full receive chains (two radios), which would not fit this board size.
- **Target size stays close to the original** 10.0 x 11.5 mm.
- **Firmware:** the plan is the stock ExpressLRS Unified firmware, with a modified `hardware.json` that adds the antenna-switch control pin. No custom firmware code is planned.

### Why not just add a second connector in parallel?

Two antennas wired directly together present roughly a 25 Ω load to a 50 Ω radio, which causes impedance mismatch. The two antenna signals can also cancel each other depending on their relative phase. A switch connects only one antenna at a time and avoids both problems.

### Known tradeoffs

- The switch adds **insertion loss** (signal lost passing through it), which slightly reduces effective output power and receive sensitivity.
- Only one antenna is active at any moment, so switching happens after the signal degrades rather than choosing the best antenna in advance.
- Single band (2.4 GHz only). ExpressLRS Gemini mode needs a true-diversity receiver, so this board would run in Basic mode.

## Status / roadmap

- [x] Research base design and ExpressLRS diversity configuration
- [ ] Choose RF switch (comparing insertion loss, isolation, control logic, package, truth table)
- [ ] Choose a free, non-strapping ESP32-C3 GPIO for switch control
- [ ] Schematic changes (switch, DC-blocking capacitors if required, second U.FL)
- [ ] Second 50 Ω RF trace on the 6-layer stackup
- [ ] Layout and design rule checks
- [ ] Fabricate, assemble, flash and bench test
- [ ] Range / link-quality comparison against the stock board

## Specifications (base board; fork values TBD)

| | |
|---|---|
| Band | 2.4 GHz |
| Radio | SX1281 |
| Antenna | U.FL (fork: 2x U.FL, switched) |
| Telemetry power | 13 dBm (20 mW) before switch loss |
| Protocol | CRSF |
| MCU | ESP32-C3 |
| Input | 5 V pad |
| Firmware | ExpressLRS |
| Flashing | Betaflight passthrough or Wi-Fi |
| Dimensions | 10.0 x 11.5 mm (original); fork TBD |
| PCB | 6-layer, 1.0 mm |

Technical write-up, part list and layout constraints: [AGENTS.md](https://github.com/KardalHaist/OpenRX-Lite-UFL_RF_Switching/blob/main/AGENTS.md).

## Contributing

See [CONTRIBUTING.md](https://github.com/KardalHaist/OpenRX-Lite-UFL_RF_Switching/blob/main/CONTRIBUTING.md).

## License

Hardware licensed under [CERN-OHL-S-2.0](https://ohwr.org/cern_ohl_s_v2.txt), see [LICENSE](https://github.com/KardalHaist/OpenRX-Lite-UFL_RF_Switching/blob/main/LICENSE). This is a derivative of OpenRX Lite-UFL by OpenDrone-hw; the original license and attribution are retained.
Hardware licensed under [CERN-OHL-S-2.0](https://ohwr.org/cern_ohl_s_v2.txt),
see [LICENSE](LICENSE).
