# plotbot
cnc writer 

## What is it?

PlotBot is a desktop pen plotter. Two NEMA 17 stepper motors drive a pen carriage in X and Y using GT2 belts, while a micro servo lifts and lowers the pen. An Arduino Uno with a CNC shield runs GRBL firmware, so you can send it G-code generated from any SVG (via Inkscape or similar).

**Specs** (update with your real numbers)

| | |
|---|---|
| Drawing area | ~ 200 mm × 250 mm (design target, update from CAD) |
| Motion | Cartesian (belt-driven X and Y) |
| Motors | 2 × NEMA 17, 12 V |
| Drivers | 2 × TMC2208 on an Arduino CNC shield |
| Pen lift | SG90 micro servo |
| Controller | Arduino Uno (ATmega328P) |
| Firmware | GRBL 1.1 (servo-capable fork) |
| Input | USB serial → G-code from SVG |
| Power | 12 V, 2 A+ |

## How to use it

1. Mount the frame on a flat board and tape paper to the drawing area.
2. Insert a pen into the holder and adjust so the tip touches the paper when the servo is down.
3. Power the CNC shield with 12 V and connect the Uno to your computer over USB.
4. Create or open an SVG in Inkscape, then export G-code (see `firmware/` for the GRBL config and Inkscape extension notes).
5. Send the G-code with a sender such as Universal G-code Sender, then watch it draw.
6. Home the machine first (limit switches on X and Y) so the origin is consistent.

## Why I made it

always struggled with writing notes and assignments


---

## Screenshots

### Full 3D assembly
![Full assembly](images/assembly.png)

### PCB
No custom PCB in this design. The electronics are an off-the-shelf Arduino Uno + CNC shield, shown in the wiring diagram below.

### Schematic / wiring diagram
![Wiring diagram](images/wiring.svg)

### 3D render (optional)
![Render](images/render.png)

---

## Repository layout

```
.
├── README.md
├── bom.csv                  # Bill of materials, with links
├── images/                  # Screenshots, renders, wiring diagram
│   └── wiring.svg
├── cad/
│   ├── PlotBot.f3d          # Full assembly source (Fusion 360) incl. electronics
│   ├── stl/                 # Print-ready STL files
│   └── step/                # One .step file per modeled part
├── firmware/
│   ├── plotbot/plotbot.ino  # Arduino sketch (flash this)
│   ├── grbl-config/         # Optional: GRBL settings ($ values)
│   ├── inkscape-extension/  # SVG → G-code notes
│   └── README.md            # How to flash and test
└── docs/
    ├── assembly-notes.md
    └── submission-checklist.md
```

> Source assembly: **Fusion 360 (.f3d)**, or swap in `.FCStd` / an Onshape link: TODO.

## Parts & BOM

Full list with links is in [`bom.csv`](bom.csv). Parts I already own are marked as such in the file.

**Estimated cost:** about **$95** for the parts still to buy (rough estimate, see the cost column in the BOM; replace with real prices). I own none of the hardware yet. The 3D printed parts are already printed.

### 3D-printed parts
All printed parts are already printed. STL files are in `cad/stl/` and STEP files in `cad/step/`.

| Part | File |
|---|---|
| Base end (powered / motor side) | `BaseEnd-Powered.STL` |
| Base end (idler side) | `BaseEnd-Idle.STL` |
| Pen slider | `PenSlider.STL` |
| Y pen end | `Y-Pen-End.STL` |
| Y servo housing | `Y-Servo-Housing.STL` |
| Servo arm | `ServoArm.STL` |
| Electronics housing | `ElectronicsHousing.STL` |
| Electronics housing lid | `ElectronicsHousing-Lid.STL` |
| Raft mount | `RaftMount.STL` |
| Finger dial | `FingerDial.STL` |

Print settings used: PLA or PETG, 0.2 mm layers, 20–30% infill (TODO: confirm what you actually used).

### Hardware
Fasteners (M3 / M5 screws and nuts, short wood screws) are listed in the BOM.

## Electronics

- **Controller:** Arduino Uno (ATmega328P) running GRBL
- **Shield:** Arduino CNC shield with 2 × TMC2208 drivers (X and Y)
- **Pen lift:** micro servo signal on D11 (Z+ header), 5 V and GND
- **Limit switches:** 2 × contact switches on X and Y endstop headers
- **Cooling:** small 5 V 30 mm fan for the drivers
- **Power:** 12 V into the CNC shield

Pin table (GRBL 1.1 defaults on the Uno; verify against your firmware fork):

| Function | Uno pin | Shield header |
|---|---|---|
| X STEP / DIR | D2 / D5 | X driver |
| Y STEP / DIR | D3 / D6 | Y driver |
| Driver ENABLE | D8 | shared |
| X limit | D9 | X+/X- endstop |
| Y limit | D10 | Y+/Y- endstop |
| Servo PWM | D11 | Z+ |

## Firmware

The included Arduino sketch ([`firmware/plotbot/plotbot.ino`](firmware/plotbot/plotbot.ino)) reads G-code over USB and drives the two steppers and the pen servo, with homing on the limit switches. A GRBL setup is also documented as an alternative. Untested on hardware in this design submission; see [`firmware/README.md`](firmware/README.md) for flashing and first tests. Assembly notes are in [`docs/assembly-notes.md`](docs/assembly-notes.md).

## Design status / roadmap

- [ ] Mechanical design (CAD)
- [x] Wiring diagram
- [x] BOM
- [x] Firmware config (untested)
- [ ] Order parts
- [ ] Assemble and calibrate
- [ ] First plot 🎉

<!-- BUILD SUBMISSION ONLY: uncomment and fill in
## Demo video
[Watch it plot](TODO-link)

## Final product
![Built plotter](images/final-1.jpg)
![Close-up](images/final-2.jpg)
![Sample drawing](images/sample-plot.jpg)
-->

## Credits & license

- Based on [DIY Machines](https://github.com/DIY-Machines/CNC-DrawingMachine).
- Built for [Hack Club Fallen](https://hackclub.com/).
- License: MIT
