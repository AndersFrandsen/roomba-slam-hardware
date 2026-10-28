# RPLIDAR A1M8 HAT – KiCad mechanical template

A KiCad board whose **Edge.Cuts is the RPLIDAR A1M8 base-plate silhouette** with the four mounting holes, a non-electrical reference footprint, a 3D model, keep-outs and dimensions.

**Status: geometry is derived and cross-checked, not copied from a dimensioned official drawing. Verify against a real unit before ordering boards (see "Verification").**

## Files

| File | Purpose |
|---|---|
| `RPLIDAR_A1M8_HAT.kicad_pcb` | The board (KiCad 7 format; opens in KiCad 7, 8, 9) |
| `RPLIDAR_A1M8_HAT.kicad_pro`, `fp-lib-table` | Project file and project-local footprint library table |
| `RPLIDAR_A1M8.pretty/RPLIDAR_A1M8_Reference.kicad_mod` | Mechanical reference (no pads, board-only, excluded from BOM/POS); carries the 3D model |
| `RPLIDAR_A1M8.pretty/MountingHole_2.7mm_M2.5_NPTH_RPLIDAR.kicad_mod` | NPTH mounting hole for the A1M8 standoffs |
| `3dmodels/RPLIDAR_A1M8.step` | Faceted STEP of the A1M8 incl. standoffs, already in KiCad footprint coordinates (no offset/rotation needed) |
| `3dmodels/LICENSE-Apache-2.0.txt` | Licence of the mesh the STEP was converted from |

Keep the folder structure; the model path is `${KIPRJMOD}/3dmodels/RPLIDAR_A1M8.step`.

## Sources

1. **SLAMTEC LD108 "RPLIDAR A1 Introduction and Datasheet, Model A1M8", rev 3.0 (2020-10-15)**, Fig. 5-2 "Mechanical Dimensions". Used for the overall size (96.8 × 70.3 mm) and the M2.5 screw size. The dimensioned drawing in the PDF is a raster image which I could not read in my tooling, so **the hole coordinates are not taken from it** — check them against Fig. 5-2 yourself.
2. **TurtleBot4 `turtlebot4_description/meshes/rplidar.dae`** (github.com/turtlebot/turtlebot4, branch humble, Apache-2.0). A CAD-derived mesh of the A1M8 with its standoffs. The outline, hole positions, standoff length and under-body clearances were measured by slicing this mesh.
3. **SLAMTEC's own A1M8 STEP/IGS model** (offered on their download site) **could not be fetched** from this environment, so it was not compared. If you have it, compare it with the values below.

## Coordinate system

- Origin = **lidar scan/rotation axis**. KiCad aux-axis origin and grid origin are set there (sheet position 150, 75 mm).
- **+Y (down on screen) points to the motor/narrow rear**; −Y is the front of the scan head; X is the left/right symmetry axis (outline and holes are symmetric about X = 0).
- View is from the top, with the lidar standing on the **top (F.Cu) side** of the PCB on its standoffs.

## Overall dimensions (Edge.Cuts)

| | This template | SLAMTEC datasheet | Difference |
|---|---|---|---|
| Length | **96.66 mm** (Y −35.10 … +61.56) | 96.8 mm | −0.14 mm |
| Width (base plate) | **70.20 mm** (X ±35.10, at Y = −1.42) | 70.3 mm | −0.10 mm |

The outline is made of eight tangent arcs fitted to the mesh. Maximum deviation from the mesh is 0.18 mm, and the mean deviation is 0.09 mm. The arcs are, written as (centre X, Y; radius) in board coordinates:

| Arc | Centre | Radius |
|---|---|---|
| Front | (0, +75.43) | 110.53 |
| Front corners (2) | (±27.16, −27.23) | 4.34 |
| Sides (2) | (±63.62, −1.42) | 98.72 |
| Rear flanks (2) | (±50.40, −0.12) | 85.43 |
| Rear nose | (0, +41.47) | 20.09 |

## Mounting holes

| Ref | Position | X (mm) | Y (mm) |
|---|---|---|---|
| MH1 | front-left | −28.00 | −28.00 |
| MH2 | front-right | +28.00 | −28.00 |
| MH3 | rear-left | −20.00 | +42.00 |
| MH4 | rear-right | +20.00 | +42.00 |

This gives a front pair spacing of 56 mm, a rear pair spacing of 40 mm and a front-to-rear pitch of 70 mm. The values are exact round numbers in the source mesh, which suggests they are the design intent.

- **Hole: 2.70 mm NPTH**, the ISO 273 *medium* clearance for **M2.5** screws. M2.5 is SLAMTEC's stated screw size, and the mesh shows Ø2.0 tapped holes in the standoffs. If you want more positional tolerance, change it to 2.9 mm (ISO 273 *coarse*) in the hole footprint.
- **Edge web:** the hole wall is 1.85 mm from the board edge at MH1/MH2 and 2.05 mm at MH3/MH4. Check that your fab can handle this; most can.
- **Standoffs (inferred from the mesh):** 5 mm A/F hex, 25.0 mm long, female thread at the PCB end. Fasten them with M2.5 screws from the bottom side of the PCB.

## Layers

| Layer | Content |
|---|---|
| `Edge.Cuts` | RPLIDAR silhouette only (8 arcs, 1 closed contour, no holes or rectangle) |
| `F.Fab` | Base-plate outline, scan-head envelope Ø71.4, under-body housing, motor can Ø32, shaft boss, standoff hexes, scan-axis cross |
| `Dimensions` (Dwgs.User) | Overall width/length, hole spacing, hole positions from origin |
| `Notes` (Cmts.User) | Summary notes |
| `RPLIDAR_Envelope` (User.1) | Scan-head envelope (overhangs the base plate) |
| `Height_Limits` (User.2) | Component-height zones under the lidar |

## Keep-outs (rule areas)

- `KO_MH1…4_StandoffScrew`: Ø7 mm on F&B.Cu. No tracks, vias or copper pour, so the standoff on top and the screw head on the bottom do not short anything.
- `KO_MotorShaftBoss_NoTopParts`: Ø12 mm around (0, +45.5) on the top side, no footprints.

Height limits for top-side parts, measured from the mesh with 25 mm standoffs:

| Area | Clearance above PCB | Suggested max part height |
|---|---|---|
| Motor shaft boss, Ø7.8 at (0, 45.5) | 4.0 mm | none |
| Motor can and bracket, Ø32 | 5.6 mm | 5 mm |
| Housing / connector block, X ±28.5, Y −17.5…+28.5 | 11.6 mm | 11 mm |
| Front housing, Ø60 | ≈21.8 mm | — |
| Outside the lidar body | 25 mm | 24 mm |

The lidar's cable connectors are on its underside, around Y ≈ +22 to +28. Leave space there for the cable.

## Inferred values and discrepancies

Values that were **inferred, not directly specified by SLAMTEC**:

- All hole coordinates.
- The arc construction of the outline.
- The standoff length and hex size.
- All under-body heights and the motor position (centre (−0.2, 45.5) in the mesh, rounded to (0, 45.5)).
- The 2.7 mm hole diameter (my choice, for M2.5).

Discrepancies found:

1. **Width.** The mesh base plate is 70.20 mm wide, the mesh scan head is Ø71.4 mm, and the datasheet says 70.3 mm. I kept the base-plate width, so the board sits within the lidar's footprint rather than outside it. The head is drawn as an envelope on User.1.
2. **Length.** The mesh gives 96.66–96.74 mm and the datasheet gives 96.8 mm. The template is 0.14 mm short, which is within the mesh's tessellation error.
3. **Unknown provenance.** It is unknown whether the TurtleBot4 mesh came from SLAMTEC's official CAD. It matches the datasheet envelope to within about 0.15 mm.

## Verification (do this before ordering)

1. Compare the hole table above with Fig. 5-2 of the SLAMTEC datasheet.
2. Plot `Edge.Cuts`, `F.Fab` and the drill layer **1:1 on paper** (File → Print, scale 1). Check the paper scale with the 96.66 mm dimension, then lay the real A1M8 on it.
3. Measure the standoff length on your unit. Some batches or kits may differ, and this changes all the height limits.

## Licence

Copyright (c) 2026 Anders Frandsen.

This hardware design is licensed under the **CERN Open Hardware Licence Version 2 – Permissive** (CERN-OHL-P-2.0). See [`LICENSE`](LICENSE). The RPLIDAR A1M8 3D model in `3dmodels/` is third-party material under Apache-2.0. See [`NOTICE`](NOTICE) for details.

Source Location: https://github.com/AndersFrandsen/RPLIDAR_A1M8_HAT
