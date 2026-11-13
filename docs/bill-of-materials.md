# Bill of materials

Prices are approximate UK prices including VAT, checked October to November 2026. Status key: **Owned**, **Ordered**, **Selected** (chosen, not yet bought), **Planned** (needed, part not chosen), **Undecided** (options still open), **Deferred** (not needed until a later stage).

| Part | Qty | Used for | Source / notes | Approx. price | Status |
|---|---|---|---|---|---|
| Metal-gear servo, 180°, "Servo and Linkage Kit" MTR7 (MG995 class) | 3 | Shoulder, elbow, Z | Bitsbox | £5.03 each | Ordered |
| Mini servo and linkage kit MTR11 (SG90 class) | 2 | Gripper, plus one spare (plastic gears are the most likely to strip while tuning grip) | Bitsbox | £4.26 each | Ordered |
| Adafruit PCA9685 8-channel PWM/servo Featherwing | 1 | Servo driver | Hand-me-down. Its headers are fitted on the top face, so it is wired with jumpers and cannot stack. Address jumpers A0 and A2 are bridged, so the expected I2C address is 0x45 (to confirm with a scan) | n/a | Owned |
| Raspberry Pi Pico WH (Pico W with pre-soldered headers) | 1 | WiFi and I2C controller, running MicroPython | The Pi Hut or Rapid Electronics; a Pico 2 WH also works | about £6 to £7 | Selected (order later) |
| Jumper wires, female-female and female-male | 1 pack each | Pico to Featherwing: 3.3 V, GND, SDA and SCL | Check lab stock first; The Pi Hut | £1.70 a pack | Selected (order later) |
| 1 A diode (1N5817 Schottky preferred; a 1N4001 also works) | 1 | Between the servo rail and the Pico's VSYS pin | Check lab stock first; Bitsbox | £0.23 | Planned (lab stock) |
| Micro-USB data cable | 1 | Programming the Pico | Must carry data, not charge only; The Pi Hut | £1.20 | Planned (if none to hand) |
| Inline switch rated 6 A or more | 1 | Emergency stop on the servo supply | Optional; the wall socket switch does the job until then | to be decided | Deferred |
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
| Laser-cut baseboard, 6 mm plywood, MDF or acrylic, about 519 x 544 mm | 1 | Holds the arm and locates the chess board | Lab laser cutter | sheet stock | Planned |

## Notes

- **Servo dimensions.** Listings give body sizes only, not hole spacing or shaft diameter. The CAD uses placeholders until the real parts are measured. See [design notes](design-notes.md#7-open-questions).
- **Servo clones.** Several marketplace listings sold as SG92R quoted SG90-class torque (1.3 to 1.8 kg·cm). Check the torque figure on any listing before buying, and prefer retailers that publish specifications.
- **Power.** The servos run at 5 V from one 5 V 6 A supply, which also powers the Pico. Both servo types are rated from 4.8 V, so this costs roughly 10 to 15 % torque and speed compared with 6 V, which the SCARA layout can afford because the bearings carry the arm's weight. The supply goes into the Featherwing's screw terminal, with the 1000 µF capacitor across it. The Pico's VSYS pin is fed from that terminal through a diode (a spare servo header's middle pin is the same rail). The diode stops USB power flowing into the servo rail when the supply is off; it drops about 0.3 V (Schottky) or 0.7 V (1N4001), and VSYS accepts 1.8 to 5.5 V. The Pico's 3V3(OUT) pin powers the Featherwing's logic. Never connect 5 V to a 3.3 V pin. If testing shows the Z axis needs more torque, a 12 V supply with an XL4015 buck set to 6 V is the fallback.
- **Rejected power options.** The RobotShop 6 V 5 A supply cost £17.53 plus £24 delivery. The spare Sagemcom 12 V 3 A router adapter uses a proprietary keyed plug rather than a standard barrel.
- **Board size.** The Puecrof squares measure 40.125 mm, so `board_size` is 321 mm. That gives a link overall length of 291.8 mm, inside the Mark 2's 315 mm limit.
