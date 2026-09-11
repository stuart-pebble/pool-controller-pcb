# Project libraries

`PoolController.kicad_sym` contains the single BAT54 clamp symbol. Its geometry is adapted from the KiCad 9 `Diode:BAT54W` symbol by the KiCad library contributors; the name, description, datasheet and default footprint were changed to the Nexperia BAT54 SOT-23 part. Pin 1 is anode, pin 3 cathode and pin 2 absent/unconnected.

This derived symbol library uses [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) with the [KiCad libraries exception](https://www.kicad.org/libraries/license/) for electronic designs and generated files. The hardware design retains the repository's CERN-OHL-P-2.0 licence.

`PoolController.pretty/RJ12_5324_6P6C.kicad_mod` preserves the custom connector geometry already embedded in this project's PCB. Bundling it makes library resolution reproducible; it does not identify or approve a substitute connector. See the revision 15 assembly notes for procurement requirements.
