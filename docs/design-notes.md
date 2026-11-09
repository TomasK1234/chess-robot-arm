# Design notes

Working notes behind the numbers in the [README](../README.md). Everything here is a design estimate until the parts are built and measured. Items that still need verifying are listed under [Open questions](#7-open-questions).

## 1. Layout parameters

The layout lives in one CAD file (`arm_layout`) where every dimension derives from the board size, so choosing a different board only means changing `board_size`. `board_size` is the **playing area** (8 squares), not the outside of the wooden board.

| Parameter | Expression | Value |
|---|---|---|
| `board_size` | `8 * 40.125 mm` (measured) | 321.0 mm |
| `square_size` | `board_size / 8` | 40.125 mm |
| `base_setback` | `board_size / 6` | 53.5 mm |
| `max_extension` | 0.9 (unitless) | 0.9 |
| `far_reach` | `sqrt(((board_size - square_size) / 2)^2 + (base_setback + board_size - square_size / 2)^2)` | 381.2 mm |
| `link1_length` | `far_reach / (2 * max_extension)` | 211.8 mm |
| `link2_length` | `link1_length` | 211.8 mm |
| `king_height` | 77 mm (measured) | 77 mm |
| `lift_height` | `king_height + carry_clearance` | 82 mm |
| `board_border` | 29 mm (measured) | 29 mm |
| `board_thickness` | 24 mm (measured, not yet used) | 24 mm |

The origin of the layout sketch is the shoulder axis. The playing area is centred left-to-right on it, with its near edge `base_setback` in front of it. All heights are measured from the playing surface (z = 0); the baseboard is currently drawn on the same plane, so `board_thickness` still has to be added when the pedestal is designed.

The physical board is 378 mm square (321 mm + 2 × 29 mm border), so its near edge is only `board_edge_clearance = base_setback - board_border` = **24.5 mm** in front of the shoulder axis. The front of the pedestal has to stay inside that.

### Measured chess set (Puecrof, November)

Heights are above the board surface. Diameters of the round pieces were measured by wrapping paper (about ±0.3 mm). The knight is not round, so it was measured straight across with a ruler, front-to-back × side-to-side. The board is 24 mm thick unfolded.

| Piece | Height | Neck height | Neck dia. | Widest above neck | Base dia. |
|---|---|---|---|---|---|
| King | 77 | 42 | 10.5 | 16.9 | 28.65 |
| Queen | | 41 | 12.25 | 18.8 | 27.7 |
| Bishop | | 34 | 9.55 | 17.2 | 23.9 |
| Knight | | 37 | 11 × 9 | 25 × 10 | 22.3 |
| Rook | | 29 | 9.4 | 16.9 | 23.9 |
| Pawn | | 19 | 8.6 | 14.0 | 19.4 |

### Gripper sizing from the measurements

**Design for the knight's 25 mm side.** The arm has no wrist servo, so the jaws always face along link 2, whose direction is `theta1 + theta2`. Across the 64 squares that direction swings through about 126° (from about 4° to 130°). Even a knight that is never turned meets the jaws at a different angle on almost every square, so the open jaws must clear the 25 mm side. Relying on the 10 mm side would need a fourth servo to rotate the gripper.

Clearance check (assumptions: ±2 mm landing error after calibration, 5 mm thick jaw fingers):

- Jaw gap when open: 25 + 2 × 2 = **29 mm**. Outer width across both fingers: 29 + 2 × 5 = 39 mm.
- Worst case is a neighbour on the next square straight across (40.125 mm away), with the widest part of any piece (the 28.65 mm king base): 40.125 − 39 / 2 − 28.65 / 2 ≈ **6.3 mm** clearance. Diagonal neighbours are about 57 mm away.
- The 6.3 mm also has to absorb pieces placed off-centre by people, so it is the margin to watch when the first gripper is tested.

Closing: the smallest neck is the pawn's 8.6 mm (the knight's is 11 × 9 mm), so the jaws must close to **7 to 8 mm or less**. V-shaped jaws also centre the knight's oval neck.

## 2. Reachability

A two-link arm can reach any point between `|L1 - L2|` and `L1 + L2` from the shoulder. With **equal links** the inner limit is zero, so there is no dead zone around the base and the squares nearest the arm are reachable.

`max_extension` keeps the farthest square at 90% of full stretch. Near full extension the arm is least stiff and the inverse kinematics become numerically unstable, so the arm should never need to work there.

### Inverse kinematics

For a target at distance `r` from the shoulder, direction `phi` (counter-clockwise positive), and equal links of length `L`:

```
cos(theta2) = (r^2 - 2 L^2) / (2 L^2)          theta2 = elbow bend, 0 = straight
theta1      = phi - atan2(L sin(theta2), L + L cos(theta2))
```

Using `theta2 > 0` gives the elbow-right configuration used throughout. Sticking to one configuration avoids the solver switching sides mid-move.

### Results for the 321 mm playing area

| Target | Distance from shoulder | Elbow bend |
|---|---|---|
| Far corner square | 381.2 mm | about 52° |
| Board centre | about 214 mm | about 119° |
| Near corner square | about 159 mm | about 136° |
| Nearest centre squares | about 76 mm | about 159° |

Link 1 (shoulder) needs to swing from about **4° to 130°** measured clockwise from straight ahead, roughly 126° in total. Both joints fit comfortably in a 180° servo.

### Clearance zones (elbow-right configuration)

- The elbow swings up to about 51 mm beyond the edge of the playing area on the elbow side (about 22 mm beyond the wooden border).
- At the near corner on the elbow side, the elbow sits about 137 mm behind the shoulder line.

Both areas of desk need to stay clear.

## 3. Precision

- **Resolution.** The PCA9685 is 12-bit over a 20 ms period, about 4.9 µs per step. Over a 1000 to 2000 µs servo range that is roughly 205 steps across 180°, or **about 0.88° per step**. At 381 mm reach that is about 6 mm at the gripper.
- **Backlash.** Metal-gear hobby servos typically have 0.5 to 1° of slop (not yet measured on these units). That is another 4 to 7 mm at full reach.
- **Tolerance.** Squares are 40.1 mm wide, so the gripper has roughly ±16 mm of tolerance to land in the square. Clearance to neighbouring pieces (about 11 mm between king bases) is the tighter limit.

Mitigations:

1. **Record servo counts per square.** Quantisation error is repeatable, so a calibration table that stores the raw counts for every square cancels it. Only repeatability matters.
2. **Approach every square from the same rotational direction.** The gear teeth stay loaded on the same face, which removes most backlash.
3. **Optional gear reduction at the shoulder.** A 2:1 or 3:1 belt or gear stage divides both resolution error and backlash by the ratio, at the cost of speed and range, both of which have spare capacity.

## 4. Loads and bearings

Rough estimates, assuming about 330 g of servos, links, gripper and a held piece beyond the shoulder:

| Joint | Static moment at full stretch |
|---|---|
| Shoulder | about 10 kg·cm |
| Elbow | about 4 kg·cm |

Dynamic torque for a 90° sweep in about 1.5 s comes out near 1.5 kg·cm with a safety factor, comfortably inside the rated torque of the metal-gear servos. The static moment, though, is a sideways load on a hobby servo's output shaft, which is not built for it. Each rotary joint therefore uses a bearing that carries the weight, with the servo only turning it.

**Chosen: 3 inch square lazy-susan bearings** (76.2 mm square, 7.9 mm thick, 31.75 mm bore, 200 lb axial rating). The tipping load is small compared with the rating. The 4 inch size was rejected: its rotating plate sweeps about 72 mm in radius, which leaves no clearance for the gripper at the nearest squares (about 80 mm from the shoulder, with a gripper roughly 30 mm wide). The 3 inch plate sweeps about 54 mm.

Expect some play. The bearings are pressed steel with small gaps in the ball track. Because the arm always leans the same way, the bearing settles into a steady tilt that calibration can absorb. If the arm is still too sloppy at full reach, the upgrade path is a thicker bearing with a larger ball track, or two bearings stacked apart.

## 5. Vertical stack (second pass, parametric)

The stack is now built from the top down in `arm_layout` parameters. Chess set values are now measured; servo, bearing and gripper values marked EST are still estimates. Changing any of them updates every level below.

| Level | Parameter | Expression | Value |
|---|---|---|---|
| Carried piece base | `lift_height` | `king_height + carry_clearance` | 82 mm |
| Underside of link 2 | `link2_bottom` | `lift_height + grip_height_max + gripper_stack + z_clearance` | 164 mm |
| Top of link 1 | `link1_top` | `link2_bottom - bearing_thickness` | 156.1 mm |
| Underside of link 1 | `link1_bottom` | `link1_top - link_thickness` | 148.1 mm |
| Top of pedestal plate | `shoulder_plate_top` | `link1_bottom - bearing_thickness` | 140.2 mm |
| Shoulder servo body base | `pedestal_height` | `shoulder_plate_top - top_plate_thickness - mtr7_height` | 88.7 mm |

Inputs: `grip_height_min` 19 mm (pawn neck, measured), `grip_height_max` 42 mm (king neck, measured), `gripper_stack` 35 mm (EST, grip point to top of gripper carriage), `link_thickness` 8 mm (EST), `top_plate_thickness` 6 mm (EST).

Each piece is gripped at its own neck height, from a calibration table. The Z axis therefore has to travel from gripping a pawn at the board up to carrying a king: `z_travel = lift_height + grip_height_max - grip_height_min`, now **105 mm**. That is more than the 80 mm in the first pass, which assumed one grip height for every piece. A rack and pinion on a 180° servo needs a pinion pitch radius of about 33 mm for that travel.

The Z guide rods are planned to be fixed to the gripper carriage and slide through bushings in the wrist hub. When lifted, they stick up above link 2, so nothing fixed hangs below link 2 except the gripper.

Checks built into the parameters:

- `elbow_servo_clearance = link1_bottom - mtr7_height - king_height`, currently 25.6 mm. If the elbow servo hangs under link 1, it clears a standing king.
- `link_overall_length = link1_length + 2 * link_end_margin`, currently 291.8 mm. It must stay within `print_max_length` (315 mm, Markforged Mark 2).
- `shoulder_keepout_radius = bearing_size / 2 * sqrt(2)`, currently 53.9 mm. The bearing's corner sweep; structure on the board side stays inside it.
- `board_edge_clearance = base_setback - board_border`, currently 24.5 mm. Shoulder axis to the near edge of the wooden board; the pedestal front must stay inside it.

The laser-cut baseboard is `baseboard_width` x `baseboard_depth`, currently 519 x 543.5 mm, and is drawn in the `baseboard` sketch. Both now include `board_border`, so the whole wooden board and the capture strips beside it fit on the baseboard.

## 6. Alternatives considered

| Idea | Outcome |
|---|---|
| Drone using the servo driver | Dropped. Multirotors need brushless motors and ESCs, not servos, and I2C timing is unsuited to flight control. |
| Desk-organising robot | Kept as a possible second project on the same platform. Too open-ended as a first build. |
| XY gantry | Rejected as bulky and ugly on a desk. |
| Articulated (human-style) arm | Rejected. The shoulder holds the whole arm's weight against gravity, and reach errors become height errors. |
| Electromagnet end effector | Rejected for a real set. It suits flat-topped pieces, but standard pieces have uneven tops. |
| FPGA servo controller | Rejected. The PCA9685 already generates PWM in hardware; an FPGA adds a steep learning curve for no benefit with four servos. |
| USB-to-I2C adapter with no microcontroller | Rejected. Every servo step would round-trip over USB, risking jerky motion. |
| USB serial link to the arm | Replaced by WiFi, so only a power cable reaches the arm. |

## 7. Open questions

- [ ] Measure the real MTR7 servos. The listing's 54 mm length is probably tab-to-tab, not body length, so check body length, mounting hole spacing (placeholder 20 mm) and shaft diameter (placeholder 6 mm).
- [ ] Measure the real MTR11 servo (placeholders: 28 mm hole spacing, 4.6 mm shaft).
- [ ] Verify the bearing's hole pattern. One listing quotes a 65 mm circle, and it is not clear whether that applies to both plates.
- [ ] Design the gripper and the Z mechanism, and check how much the upper hardware limits the lift height.
- [x] Check grip height on the necks of real Staunton pieces across the set (see the measured table in section 1).
- [x] Choose and buy the board and set together. King height drives the whole vertical stack.
- [x] Re-measure the knight neck and head straight across with a ruler (neck 11 × 9 mm, head 25 × 10 mm).
- [x] Confirm that the 24 mm board thickness is with the board unfolded (yes).
- [ ] Check that the pedestal fits inside the 24.5 mm between the shoulder axis and the board edge, or increase `base_setback`.
- [ ] Decide the ESP8266 board and the power supply (see the [bill of materials](bill-of-materials.md)).
- [ ] Confirm in the 3D model that link 2 clears link 1 and the shoulder structure at the roughly 160° elbow fold.
