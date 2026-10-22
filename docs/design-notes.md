# Design notes

Working notes behind the numbers in the [README](../README.md). Everything here is a design estimate until the parts are built and measured. Items that still need verifying are listed under [Open questions](#7-open-questions).

## 1. Layout parameters

The layout lives in one CAD file (`arm_layout`) where every dimension derives from the board size, so choosing a different board only means changing `board_size`.

| Parameter | Expression | Value |
|---|---|---|
| `board_size` | 350 mm | 350.0 mm |
| `square_size` | `board_size / 8` | 43.75 mm |
| `base_setback` | `board_size / 6` | 58.33 mm |
| `max_extension` | 0.9 (unitless) | 0.9 |
| `far_reach` | `sqrt(((board_size - square_size) / 2)^2 + (base_setback + board_size - square_size / 2)^2)` | 415.7 mm |
| `link1_length` | `far_reach / (2 * max_extension)` | 230.9 mm |
| `link2_length` | `link1_length` | 230.9 mm |
| `king_height` | 76 mm | 76 mm |
| `lift_height` | `king_height + 5 mm` | 81 mm |
| `pedestal_height` | 70 mm | 70 mm |

The origin of the layout sketch is the shoulder axis. The board is centred left-to-right on it, with the board's near edge `base_setback` in front of it.

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

### Results for the 350 mm board

| Target | Distance from shoulder | Elbow bend |
|---|---|---|
| Far corner square | 415.7 mm | about 52° |
| Board centre | about 233 mm | about 119° |
| Near corner square | about 173 mm | about 136° |
| Nearest centre squares | about 80 mm | about 160° |

Link 1 (shoulder) needs to swing from about **4° to 130°** measured clockwise from straight ahead, roughly 126° in total. Both joints fit comfortably in a 180° servo.

### Clearance zones (elbow-right configuration)

- The elbow swings up to about 55 mm beyond the board's right edge.
- At the near corner on the elbow side, the elbow sits about 150 mm behind the shoulder line.

Both areas of desk need to stay clear.

## 3. Precision

- **Resolution.** The PCA9685 is 12-bit over a 20 ms period, about 4.9 µs per step. Over a 1000 to 2000 µs servo range that is roughly 205 steps across 180°, or **about 0.88° per step**. At 416 mm reach that is about 6 mm at the gripper.
- **Backlash.** Metal-gear hobby servos typically have 0.5 to 1° of slop (not yet measured on these units). That is another 4 to 7 mm at full reach.
- **Tolerance.** Squares are 43.75 mm wide, so the gripper has roughly ±18 mm of tolerance.

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

## 5. Vertical stack (first pass)

Assumes a set with a king of about 76 mm, which usually pairs with squares of about 44 mm.

| Level | Height above board | Why |
|---|---|---|
| Tallest piece (king) | 76 mm | Sets everything else |
| Lift needed to carry a piece | about 80 mm | The carried piece's base must clear the king |
| Link 2 (underside) | about 155 mm | Lifted piece's neck at about 120 mm plus about 35 mm of gripper hardware |
| Link 1 (top surface) | about 125 mm | Link 2 sits about 30 mm above it, on the elbow servo and horn |
| Shoulder servo (base) | about 70 mm | Servo is about 45 mm tall; link 1 attaches to its horn |

The shoulder servo hangs under a top plate, with its shaft passing up through the plate and the bearing's centre hole. That plate ends up roughly 110 mm above the board, held on posts around the servo. All of these numbers will move once the gripper and Z mechanism are designed.

The Z axis is planned as a servo-driven crank and slide with about 80 mm of travel, which keeps it on the same PCA9685 as the other joints.

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
- [ ] Check grip height on the necks of real Staunton pieces across the set.
- [ ] Choose and buy the board and set together. King height drives the whole vertical stack.
- [ ] Decide the ESP8266 board and the power supply (see the [bill of materials](bill-of-materials.md)).
- [ ] Confirm in the 3D model that link 2 clears link 1 and the shoulder structure at the roughly 160° elbow fold.
