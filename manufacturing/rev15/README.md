# Rev15 prototype manufacturing package

**Use for a sample quotation/build after resolving the assembly sourcing holds.** Read [assembly and acceptance requirements](../../docs/rev15-assembly.md) with the supplier. No physical prototypes have yet been validated for this revision.

- `fabrication.zip`: two copper layers, mask, silkscreen, paste, outline, Gerber job file and separate plated/non-plated Excellon drills. Order **80 × 75 mm, 1.2 mm FR-4, nominal 1 oz copper per side**. Approve finish, panel tooling and thermal-pad via treatment with the assembler.
- `BOM.csv`: 19 carrier components (15 SMT and 4 through-hole). J1/J2 have an explicit procurement hold because the original connector has no traceable MPN. L1 stock/lifecycle requires confirmation.
- `CPL.csv`: native KiCad CSV placement data, **millimetres, absolute board origin**, front side for all 19 references. Negative Y coordinates follow KiCad's exported Cartesian convention; no origin translation or rotation correction has been applied. Includes through-hole parts. Map `Ref/PosX/PosY/Rot/Side` to the supplier's placement fields and approve every orientation in their preview, especially D3, Q1, U1, RJ12 and J4. Footprint origins are the source placement references; the assembler may adjust feeder/centroid conventions.
- `assembly.pdf`, `schematic.pdf`, `copper.pdf`: reference drawings (front assembly, circuit, and front/back copper respectively).
- `erc.json`, `drc.json`: native KiCad checks including schematic parity, required to pass before export.
- `manifest.json`: KiCad version and SHA-256 fingerprints of source and order artifacts. Regenerate whenever the design changes; do not mix revisions.

The **Waveshare ESP32-C6-Zero development board is additional to this BOM**, which contains only its socket. Include supply, headers, correct-end insertion, programming and functional tests as separate quote items if assembled working units are required. Do not assume they are included in automated SMT assembly.

## Regeneration

Use KiCad 9 with its standard symbol/footprint libraries installed, plus Python 3. Refill zones and save the PCB in KiCad, then run from the checkout:

```sh
scripts/export-manufacturing.sh
# Or select a specific KiCad 9 CLI:
KICAD_CLI=/path/to/kicad-cli scripts/export-manufacturing.sh
```

The script requires clean ERC, DRC and schematic parity, checks the 19-reference BOM/placement set and selected revision-specific MPNs, then replaces the generated artifacts. It does not order boards or certify electrical behavior. Timestamps and PDF metadata can differ between export runs; use the manifest to identify this exact package.
