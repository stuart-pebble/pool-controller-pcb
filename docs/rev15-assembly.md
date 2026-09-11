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

[PCBWay supports mixed SMT/through-hole assembly](https://www.pcbway.com/assembly-capabilities.html) and [turnkey, nominated-distributor, consigned or combined sourcing](https://www.pcbway.com/pcb_prototype/Electronic_Components.html). Request **turnkey PCB fabrication plus mixed assembly** with the PCBWay-specific BOM and SMT centroid in the manufacturing folder. PCBWay's published capabilities support these package types; only their reviewed quote can confirm every exact part, lead time, reflow profile and manual installation service. No order or quote has been submitted.

### RJ12 sockets: specified distributor part

J1/J2 are **Ckmtw R-RJ11R06P-A000**, **LCSC C2902699**, quantity **2 per carrier**. [Distributor product/source](https://www.lcsc.com/product-detail/C2902699.html), [manufacturer drawing through LCSC](https://www.lcsc.com/datasheet/C2902699.pdf). Although marketed as RJ11, the drawing explicitly specifies six positions and six contacts (6P6C), suitable for the requested RJ12 connection. Do not substitute a four-contact socket.

The board now uses a dedicated project footprint derived from drawing **C2902699, revision A**: 1.27 mm contact progression, 2.54 mm row spacing, six 0.90 mm plated holes, and two 3.20 mm non-plated retention holes spaced 10.16 mm. Contacts 1/6 carry supply, 2/3/5 ground, and 4 bus data, preserving schematic numbering. Connector-region supply/data routing and ground connections were updated for the new holes.

This is a **larger, tab-down connector**, approximately 13.21 mm wide × 20.54 mm deep × 12.70 mm high. It replaces the original 5324 mini geometry; the two parts are not interchangeable. Connector origins are now J1 (82.50, 75.00) and J2 (82.50, 93.55) mm, rotated −90°. The mating face overhangs the left PCB edge. The body-to-tail offset is not dimensioned in the supplier drawing: the fabrication outline is an approximate envelope and the courtyard allows extra clearance. Check the actual part against the enclosure and front opening before committing enclosure manufacture. Silkscreen is clipped back inside the PCB and does not represent the overhanging face.

LCSC displayed stock at review on 2026-09-12; stock is not reserved and PCBWay must confirm procurement. The drawing calls out a 1.6 mm PCB, whereas this carrier retains its required **1.2 mm** thickness. Ask PCBWay to confirm the snap-peg seating/retention on 1.2 mm and hold the connector flush during soldering if necessary. Through-hole solder joints and the final mechanical fit require inspection. The obsolete approximate Wayconn 3D model is not used for these connectors.

### Full-BOM sourcing check for PCBWay

The BOM now includes nominated supplier links for every carrier MPN. These are evidenced procurement routes, not a PCBWay stock commitment. See [procurement data](../manufacturing/procurement.json) for the exact links used by the exporter.

- **C1, C2/C3, C5 and L1** have NRND/end-of-life warnings despite distributor stock. Confirm allocation of the exact parts. Power-stage substitutions need separate electrical review.
- **J4 DILB24P-223TLF is obsolete**, with remaining distributor stock at review. Confirm availability and actual contact finish; the distributor describes tin-lead, so do not assume a lead-free assembly from the suffix. If PCBWay requires a RoHS build or stock is exhausted, qualify a current socket with the same dimensions and compatible contacts before release.
- **D1/D2** have a stocked LCSC route (C110797) despite DigiKey backorder. **R3** has a Mouser route despite DigiKey backorder. The other BOM lines have distributor listings with stock at review. PCBWay may need procurement minimums and assembly overage beyond fitted quantities.
- Approve U1's reflow limit and thermal-pad via treatment, as above. Confirm tooling/fiducials with PCBWay; mounting holes are not an approved tooling-hole pattern.

### Waveshare module: already purchased by customer

The customer has already ordered the finished **Waveshare ESP32-C6 Mini Development Boards** separately. **Do not procure additional modules.** The PCBWay purchasing BOM includes only the carrier's **DILB24P-223TLF socket**, not a Waveshare board. The default build scope is a fully soldered carrier; the customer fits their module after delivery.

If PCBWay is to install the customer-owned modules, arrange **combo/consigned assembly** and quote insertion separately before shipping them. Confirm the exact supplied variant and header state: Waveshare's **ESP32-C6-Zero-M** has presoldered headers, while the unheaded variant needs compatible headers fitted first. The already-ordered variant has not been independently identified. [Waveshare product and variants](https://www.waveshare.com/product/arduino/esp32-c6-zero.htm). Module programming and functional tests are not included without an agreed firmware version and test procedure.

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
