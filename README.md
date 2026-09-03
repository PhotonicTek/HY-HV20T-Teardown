# DY-HV20T — Teardown Documentation

> A complete teardown log of a DY-HV20T audio playback module. Everything here was produced by physically desoldering, tracing, and analysing an original board. use at your own risk.

---
![Main](/photos/Banner_Photo.png)
---

## Background

The DY-HV20T is a small, inexpensive audio playback board available from the usual Chinese electronics suppliers. It's surprisingly capable, but its public documentation for it is essentially nonexistent.

This repository is the result of a full physical reverse engineering effort: every component was desoldered, identified, and measured; both copper layers were imaged via  flatbed scanning and laser ablation; and the full circuit was traced and redrawn as a schematic from scratch.

---

## The ICs — What's Identified and What Isn't

### DY1900A (Main Logic IC)
This is the main IC of the board and the one part that remains a black box. i was unable to find a datasheet or public reference. It's either a custom-marked microcontroller made for this PCB, or a part that only circulates in the Chinese domestic market.

From the board layout we can infer:
- Handles audio playback and decoding
- Contains onboard flash memory and RAM
- Manages IO (volume control, trigger inputs, DIP switch config)
- Likely has USB capability.

**Practical implication:** This IC cannot be substituted. Any board built from this documentation needs a DY1900A harvested from an original module. 

Pinouts are as follows referenced from the PCB.
![DY1900A](/photos/DY1900A_Pinouts.jpg)


### AP2001D (Audio Amplifier)
Unfortunately was also unable to find an exact datasheet for this IC eithor, see reason above. From teardown analysis and comparison with known equivalent ICs (such as the XPT8871 family which shares the same 8-pin SOP footprint and pin functions.)

From the board layout we can infer:
- Mono BTL (Bridge-Tied Load) amplifier, single channel in, single speaker output
- Runs off 6–12V on the high side power rail,
- Pins 1–3 are likely Shutdown, Bypass, and Mode select respectively, this is adopted from the XPT8871 equivalent pinout
- In this circuit, L and R channels from the MCU are mixed passively via the 3.5mm jack switch contacts when no headphones are plugged in, fed through a 10kΩ volume potentiometer (R28) before entering the amp

Pinouts are as follows referenced from XPT8871.
![DY1900A](/photos/AP3001D_Pinouts.jpg)

### Buck Converter
Standard off-the-shelf step-down converter. 
- Datasheet can be found here: [Link](https://www.umw-ic.com/static/pdf/c33962bfffc11885f082e362d8a545a5.pdf)


---

## Schematics

Schematic files are in [`/schematics`](/schematics).

- Source files are in EasyEDA JSON format (importable via File → Import in EasyEDA)
- PDF and SVG exports are included for viewing without the program.

The schematic covers the full original DY-HV20T circuit including the audio amplifier stage, power supply, IO headers, potentiometer, DIP switch config, and USB pads.

![Clean Schematic](/schematics/Sheet_2.png)

---

## Parts List

Full BOM is in [`/parts-list/bom.csv`](/parts-list/BOM.csv).

Key notes:
- Linked parts are my best guess and may not be identical, please do your own research before committing to buying, as better prices or closer-matched parts may be available.
- IC sources: the Buck converter is a standard part; the DY1900A must be sourced from an original module; the AP2001D should also be sourced from an original module, however the XPT8871 could work as a substitute.
- Generic resistors and caps can be substituted freely with any standard 0603 1% tolerance parts at the correct value.

---

## Photos

[`/photos`](/photos) contains board photos taken throughout the process.

[`/photos/components/`](/photos/components/)
Close-up photos of all major parts removed from the PCB.

[`/photos/pcb/`](/photos/pcb/)/
- Top of PCB
- Bottom of PCB
- Top trace layer
- Bottom trace layer
- Component numbering that correlates to the BOM

V1 photos are the originals taken during teardown; V2 are cleaner photos taken after the fact.

---

## Licence

All documentation in this repository (schematics, BOM, scans, photos) is released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Use it, build on it, share it. just credit the source.

---

## AI Use Disclaimer

Portions of this documentation and formatting were drafted with the use of AI writing tools and reviewed for accuracy against the physical board. All technical content (component values, circuit tracing, IC identification, and schematic work) was produced through hands-on reverse engineering. If you spot an error, please open an issue.

---

## See Also

- Video walkthrough of the full teardown and tracing process: *[link added soon!]*
- EasyEDA (free schematic tool used here): https://easyeda.com
- Krita (free image editor used for annotation and layer overlay): https://krita.org
