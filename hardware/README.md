# Hardware

Mechanical design for the arm, modelled in Autodesk Fusion 360.

## Status

The CAD currently lives in a Fusion project, not in this repository. Exported models will be committed to [`cad/`](cad/) at each milestone (a STEP file for exchange and an F3D archive for editing). Printable STL files will be added once the design is fixed and the real parts have been measured.

## Fusion files

| File | Purpose | Status |
|---|---|---|
| `arm_layout` | Skeleton sketch of the board and the two links, driving every other dimension. Contains the layout parameters. | Done |
| `MTR7_reference` | Placeholder body of the metal-gear servo used at the shoulder, elbow and Z axis | Done (placeholder dimensions) |
| `MTR11_reference` | Placeholder body of the mini servo used in the gripper | Done (placeholder dimensions) |
| `base_mount` | Plate around the shoulder servo, being reworked into the pedestal | In progress |
| Links, elbow, Z mechanism, gripper | | Not started |

## Conventions

- **Units are millimetres.**
- **Every dimension is a named parameter.** Layout parameters derive from `board_size` (see the table in [design notes](../docs/design-notes.md#1-layout-parameters)). Servo dimensions are parameters too, named `mtr7_*` and `mtr11_*`, so measuring the real parts only means editing a few values.
- **Components have descriptive names**, not `Component1`.
- **The shoulder axis is the origin** of `arm_layout`, with the board in front of it.

## Servo parameters

The listings give body sizes only. Hole spacing and shaft diameter are placeholders until the real servos arrive and are measured.

| Parameter | MTR7 (shoulder, elbow, Z) | MTR11 (gripper) |
|---|---|---|
| Length | 54 mm (probably tab to tab) | 32 mm |
| Width | 19.5 mm | 12 mm |
| Height | 45.5 mm | 30 mm |
| Mounting hole spacing | 20 mm (placeholder) | 28 mm (placeholder) |
| Shaft diameter | 6 mm (placeholder) | 4.6 mm (placeholder) |

## Bearing parameters

3 inch square lazy-susan bearings at the shoulder and elbow: 76.2 mm square, 7.9 mm thick, 31.75 mm centre hole. The hole pattern still has to be checked on the real part.
