# Bill of materials

Prices are approximate UK prices including VAT, checked October to November 2026. Status key: **Owned**, **Ordered**, **Selected** (chosen, not yet bought), **Planned** (needed, part not chosen), **Undecided** (options still open), **Deferred** (not needed until a later stage).

| Part | Qty | Used for | Source / notes | Approx. price | Status |
|---|---|---|---|---|---|
| Metal-gear servo, 180°, "Servo and Linkage Kit" MTR7 (MG995 class) | 3 | Shoulder, elbow, Z | Bitsbox | £5.03 each | Ordered |
| Mini servo and linkage kit MTR11 (SG90 class) | 2 | Gripper, plus one spare (plastic gears are the most likely to strip while tuning grip) | Bitsbox | £4.26 each | Ordered |
| Adafruit PCA9685 16-channel servo Featherwing | 1 | Servo driver | Adafruit | n/a | Owned |
| Adafruit Feather HUZZAH ESP8266 | 1 | WiFi and I2C controller; stacks directly on the Featherwing | The Pi Hut | £14.40 | Selected (order later) |
| Feather stacking headers | 1 set | Stacking the Featherwing on the HUZZAH | The Pi Hut | £1.10 | Selected (order later) |
| 5 V 6 A DC supply, 5.5 x 2.1 mm plug, centre positive | 1 | Single supply for the servos and the logic | Amazon UK | about £10 | Ordered |
| 2.1 mm PCB DC socket (CN018) | 1 | Supply input | Bitsbox | £1.33 | Ordered |
| 1000 µF 25 V electrolytic capacitor (LC1KU25) | 1 | Bulk smoothing across the servo terminals | Bitsbox | £0.47 | Ordered |
| 3 inch square lazy-susan bearing (76.2 mm, 31.75 mm bore) | 2 | Shoulder and elbow | Bearing King (BKL brand) | £3.73 each | Ordered |
| Puecrof 15 inch folding chess set (40 mm squares, 3 inch king, 30 mm king base) | 1 | Playing surface and pieces | Amazon UK (B0DY8FTPBJ) | £25.99 | Ordered |
| M3/M4 stainless screw, nut and washer kit | 1 | Assembly | Check lab and own stock first; Taiss 405-piece kit if needed | £16.87 | Planned (before first prototype) |
| M3 heat-set inserts | about 100 | Threads in printed parts | Check lab stock first; YAMXIN 100-piece if needed | £7.99 | Planned (before first prototype) |
| 18 to 20 AWG hook-up wire | short length | Supply wiring | | about £3 | Planned (if none to hand) |
| Servo extension leads | to be decided | Elbow, Z and gripper leads to the base | Lengths set by the cable routing in CAD; buy ready-made or crimp to length | about £2 to £2.50 each | Deferred |
| USB webcam | 1 | Overhead vision | Any 1080p USB webcam, for example Trust Tanor (Argos) | £17 to £20 | Deferred (vision stage) |
| Z-axis guide rods and bushings | to be decided | Z axis | Depends on the Z design | to be decided | Deferred |
| PTFE grease | 1 | Bearings and guides | Optional | £7.49 | Deferred |
| 3D-printed parts (PLA or PETG) | n/a | Prototype links, pedestal, gripper | Lab Ultimaker S5 | filament | Planned |
| Fibre-reinforced links (Onyx + continuous carbon fibre) | 2 | Final links 1 and 2 | Lab Markforged Mark 2 (320 mm max length) | lab material | Planned |
| Laser-cut baseboard, 6 mm plywood, MDF or acrylic, about 500 x 500 mm | 1 | Holds the arm and locates the chess board | Lab laser cutter | sheet stock | Planned |

## Notes

- **Servo dimensions.** Listings give body sizes only, not hole spacing or shaft diameter. The CAD uses placeholders until the real parts are measured. See [design notes](design-notes.md#7-open-questions).
- **Servo clones.** Several marketplace listings sold as SG92R quoted SG90-class torque (1.3 to 1.8 kg·cm). Check the torque figure on any listing before buying, and prefer retailers that publish specifications.
- **Power.** The servos run at 5 V from one 5 V 6 A supply, which also feeds the Feather's USB pin. Both servo types are rated from 4.8 V, so this costs roughly 10 to 15 % torque and speed compared with 6 V, which the SCARA layout can afford because the bearings carry the arm's weight. Take the Feather's feed directly from the supply input rather than from the servo wiring, and keep the 1000 µF capacitor across the servo terminals. If testing shows the Z axis needs more torque, a 12 V supply with an XL4015 buck set to 6 V is the fallback.
- **Rejected power options.** The RobotShop 6 V 5 A supply cost £17.53 plus £24 delivery. The spare Sagemcom 12 V 3 A router adapter uses a proprietary keyed plug rather than a standard barrel.
- **Board size.** The Puecrof set has 40 mm squares, so `board_size` becomes 320 mm once a square is measured. That gives a link overall length of about 291 mm, inside the Mark 2's 315 mm limit.
