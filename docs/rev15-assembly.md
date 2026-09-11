# Revision 15 — prototype assembly and acceptance

Revision 15 implements the powered RX clamp proposed in [issue #3](https://github.com/marklynch/pool-controller-pcb/issues/3) and the 560 Ω TX base drive from [issue #4](https://github.com/marklynch/pool-controller-pcb/issues/4). It is a prototype revision: KiCad checks and manufacturing outputs do not establish operation on every Connect 10 installation.

## Circuit changes

- D3 = **Nexperia BAT54,215**, single Schottky diode, SOT-23. Pin 1/anode connects to RX; pin 3/cathode connects to the Waveshare 3.3 V rail through J4 pin 3; pin 2 is not connected. Do not substitute BAT54A, BAT54C or BAT54S dual-diode variants.
- R3 = **560 Ω, Yageo RC0603FR-07560RL**, replacing 1 kΩ. This is the TX base resistor, called R4 in older issue descriptions. Current R4 remains **100 kΩ** from base to ground.
- R1/R2 remain 100 kΩ. The clamp diverts excess RX current into the powered module's 3.3 V rail; it is not an isolated receiver or a guaranteed power-off-safe interface.
- L1 is specified as **Bourns SRR1050A-121Y**, previously one of the two listed choices. Existing pads encompass the manufacturer's recommended lands. The Bourns series is available but marked not recommended for new designs. Confirm stock before ordering; a replacement requires a separate electrical/mechanical review. The supplied WEBENCH calculation used the NIC alternative, so repeat power-supply acceptance with the actual fitted Bourns part.

## Order definition

| Item | Specification |
|---|---|
| Board | 80 × 75 mm, two layers, FR-4, **1.2 mm** thickness |
| Copper | 35 µm / nominal 1 oz per side |
| Components | Front-side SMT plus through-hole connectors/socket |
| Finish | Agree with assembler; keep board thickness at 1.2 mm for mechanical fit |
| Quantity | Small prototype batch; price depends on quantity, sourcing and manual assembly |
| Assembly process | Confirm reflow profile for LM2678S-5.0/NOPB (TI lists 245 °C peak). Do not select a hotter process without manufacturer/assembler approval. |
| U1 thermal pad | Four existing 0.8 mm drilled ground vias within the thermal pad; tenting is requested, not filled/capped vias. Assembler must approve paste and via treatment. |
| Inspection | Electrical bare-board test, assembly polarity/placement inspection, then functional acceptance below |

A combined fabrication and mixed SMT/through-hole assembly order is supported by suppliers such as [JLCPCB](https://jlcpcb.com/capabilities/pcb-assembly-capabilities). Standard assembly is the initial quote route for the listed regulator temperature limit and sourcing/manual assembly needs. Add tooling rails/fiducials as required by the selected assembler; the four mounting holes are not an approved tooling-hole pattern.

### RJ12 sockets: procurement hold

J1/J2 retain the existing **5324 mini 6P6C** connector geometry. The original sourcing reference is [this vendor listing](https://www.aliexpress.com/i/1005008617248283.html); it is not a traceable manufacturer MPN. The PCB's exact footprint is bundled in the project library for reproducibility. This does not qualify an arbitrary substitute.

Before authorizing connector procurement, have the assembler match drawings or physical samples to the supplied footprint: all six contacts, two locating pegs, body size/overhang and mating orientation. Either obtain an approved exact item or consign verified connectors. The BOM marks this hold explicitly; no invented manufacturer part number is provided.

### Waveshare module: an additional assembly item

Use the finished **Waveshare ESP32-C6 Mini Development Board / ESP32-C6-Zero**, with headers fitted for insertion into J4. This is a separate item from the **DILB24P-223TLF socket** in the carrier BOM. Agree whether the assembler supplies and installs the module or the customer plugs it in after receiving the fully soldered carrier. A carrier-only quote does not include a controller module or firmware.

Align the module's **5V pin to J4 pin 1 at the marked socket end**, GND to pin 2, 3V3 to pin 3, GPIO1 to pin 5 and GPIO2 to pin 6. The module has fewer pins than the DIP-24 socket; do not center it in the socket or shift it one position. Verify row spacing, header/socket contact compatibility, seating height, USB access and antenna clearance with the exact supplied board and enclosure. The generic socket 3D model is not a model of the Waveshare board.

Use Waveshare's [schematic and mechanical resources](https://docs.waveshare.com/ESP32-C6-Zero/Resources-And-Documents) for the exact supplied variant. Do not interchange a similarly named third-party “SuperMini” board. Supply the firmware release/version and programming requirements separately if functional units must arrive ready to run.

## Functional acceptance for samples

Perform powered tests with a current-limited source and known bus simulator before connecting a pool system. Establish the actual installation's supply/transient and data-high ranges rather than relying on the silkscreen's nominal values.

1. Inspect D3/Q1 pin orientation, all connector pins, and the module insertion. Measure absence of unintended supply-to-ground shorts before power-up. Verify RX to D3 anode, cathode to module 3V3, and continuity of the intended RJ12 pin mapping.
2. With the module removed, test the buck converter at the intended input range up to 40 V with 0–0.5 A loads. Scope output startup, ripple, load transients, ringing and component temperatures. This board is not rated for a 5 A output simply because the IC is a 5 A regulator.
3. Fit the module before applying a data signal. Scope **both RX and the 3.3 V rail** at 7 V and 12 V data-high, including resistor tolerances and actual bus source impedance. Check VIH/VIL against the ESP32-C6 datasheet and verify the clamp does not lift the rail.
4. Repeat during boot, reset, low-load/sleep states and shutdown. At nominal 12 V with RX around 3.54 V, approximately 49 µA enters the 3.3 V rail. With an unpowered rail, current can approach 0.12 mA. The design must not be treated as providing power-off isolation. If the actual operating sequence leaves a driven bus connected to an unpowered module and measurements exceed allowed limits, revise the receiver before field use.
5. Check temperatures relevant to the enclosure. The BAT54 forward-voltage bound used in the nominal analysis is a 25 °C test condition, not an all-temperature clamp guarantee.
6. Confirm TX can pull low against the real bus load at both high levels, with the exact MMBT3904 supplied. Scope low level and release edges; test sustained bidirectional traffic alongside existing devices and check for stuck-low or intermittent operation.
7. Verify pool-only, USB-only and simultaneous-power behavior with the exact Waveshare board, then Wi-Fi/firmware operation in the case. Confirm cable pin mapping before connection to actual equipment.

Do not connect the bus to an empty J4 socket or assume the clamp makes all unpowered states safe. Reject samples with unexplained rail rise, excessive RX voltage, unstable supply, communication faults or poor soldering.

## References

- [Nexperia BAT54 datasheet](https://assets.nexperia.com/documents/data-sheet/BAT54.pdf): single-diode pinning and forward characteristics.
- [Espressif ESP32-C6 datasheet](https://documentation.espressif.com/esp32-c6_datasheet_en.html): GPIO input levels relative to its supply domain.
- [Bourns SRR1050A datasheet](https://www.bourns.com/docs/product-datasheets/srr1050a.pdf): lands, lifecycle and electrical limits.
- [TI LM2678 datasheet](https://www.ti.com/lit/gpn/lm2678): power/layout and reflow requirements.
