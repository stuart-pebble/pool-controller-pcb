# PCBWay sample assembly instructions

Request a **turnkey fabrication and mixed SMT/through-hole assembly quote** using the current revision 15 files. Quantities in the BOM are **per finished unit**; select the desired sample batch quantity in the quote. Allow supplier minimums and assembly overage separately.

Upload:

1. `fabrication.zip` — 80 × 75 mm, two-layer FR-4, **1.2 mm**, nominal 1 oz copper each side.
2. `PCBWay-BOM.csv` — 19 fitted carrier parts; all exact carrier MPNs have suggested source links. Waveshare modules have already been purchased by the customer and are excluded.
3. `PCBWay-SMT-CPL.csv` — 15 SMT components, mm, absolute origin, top side; map KiCad column names if requested. Do not translate the origin independently of the fabrication files.
4. `assembly.pdf`, `schematic.pdf` and `docs/rev15-assembly.md` — reference drawings and acceptance requirements. `CPL.csv` also supplies all 19 carrier placements for engineering review.

Include J1/J2/J3/J4 through-hole fitting in the price. **Do not purchase Waveshare modules.** Deliver fully soldered carriers for the customer to fit their existing modules. If the customer instead consigns modules to PCBWay, quote insertion separately. Use Ckmtw **R-RJ11R06P-A000**, LCSC **C2902699**, for both RJ12 sockets. This package has new connector holes/routing; do not reuse the earlier revision 15 fabrication ZIP or the original 5324 mini sockets.

Before accepting the quote, obtain PCBWay's confirmation of:

- Exact MPN procurement and quantities, especially C1/C2/C3/C5/L1 lifecycle flags and the obsolete J4 socket. Do not substitute without approval; confirm J4 contact finish if lead-free compliance is required.
- Regulator peak reflow limit of 245 °C; handling of four drilled/tented, unfilled thermal-pad vias; tooling/panel rails and fiducials.
- RJ12 snap-peg fit on **1.2 mm** PCB (drawing specifies 1.6 mm). Hold parts flush during soldering as needed. Confirm larger tab-down connector bodies, front overhang and enclosure clearance. The body envelope in the drawing is approximate; the numbered hole pattern follows the manufacturer drawing.
- For any customer-consigned Waveshare modules, confirm the actual variant/header state, header/socket compatibility and correct-end insertion: 5V to J4.1, GND to J4.2, 3V3 to J4.3. Do not center the shorter module in the DIP-24 socket.
- Programming and functional testing only against an explicitly supplied firmware binary/version and test procedure. A fully soldered board is not automatically programmed or functionally accepted.

PCBWay's published sourcing and mixed-assembly capabilities support this quote route. No PCBWay review, stock reservation, exact-part acceptance or purchase is implied by these files.
