# Bill of materials

Prices are approximate UK prices including VAT, checked October 2026. Status key: **Owned**, **Selected** (chosen, not yet bought or confirmed), **Planned** (needed, part not chosen), **Undecided** (options still open).

| Part | Qty | Used for | Source / notes | Approx. price | Status |
|---|---|---|---|---|---|
| Metal-gear servo, 180°, "Servo and Linkage Kit" MTR7 (MG995 class) | 3 | Shoulder, elbow, Z | Bitsbox | £5.03 each | Selected |
| Mini servo and linkage kit MTR11 (SG90 class) | 1 | Gripper | Bitsbox | £3.98 | Selected |
| Adafruit PCA9685 16-channel servo Featherwing | 1 | Servo driver | Adafruit | n/a | Owned |
| ESP8266 board | 1 | WiFi and I2C controller | Adafruit HUZZAH (about £20) or NodeMCU / Wemos D1 Mini (about £3 to £5) | £3 to £20 | Undecided |
| 6 V DC supply, 4 to 5 A | 1 | Servo power rail | Dedicated 6 V supply, or reuse a spare 9 V 1.7 A supply with an extra buck stage | to be decided | Undecided |
| Buck converter module | 1 to 2 | Logic rail (and 6 V if reusing a 9 V supply) | | a few £ | Planned |
| Breadboard-friendly 2.1 mm DC barrel jack | 1 | Supply input | The Pi Hut / Adafruit | a few £ | Planned |
| 470 to 1000 µF electrolytic capacitor | 1 | Smoothing on the servo rail | | under £1 | Planned |
| 3 inch square lazy-susan bearing (76.2 mm, 31.75 mm bore) | 3 | Shoulder, elbow, one spare | Simply Bearings, Bearing Shop UK or Bearing King | about £4 to £5 each | Selected |
| Bearing grease or light oil | 1 | Lubricating the bearings | Any hardware or bike shop | a few £ | Planned |
| USB webcam | 1 | Overhead vision | Clipped to the monitor | £10 to £25 | Planned |
| 350 mm chess board and Staunton set | 1 | Playing surface and pieces | Buy as a pair; king no taller than about 76 mm | to be decided | Not yet bought |
| 3D-printed parts (PLA or PETG) | n/a | Links, pedestal, gripper | Designed in Fusion 360 | filament | Planned |
| Fasteners, wire, perfboard | n/a | Assembly | | a few £ | Planned |

## Notes

- **Servo dimensions.** Listings give body sizes only, not hole spacing or shaft diameter. The CAD uses placeholders until the real parts are measured. See [design notes](design-notes.md#7-open-questions).
- **Servo clones.** Several marketplace listings sold as SG92R quoted SG90-class torque (1.3 to 1.8 kg·cm). Check the torque figure on any listing before buying, and prefer retailers that publish specifications.
- **Power.** USB cannot supply the servos: three MTR7 servos can draw 1.5 to 2 A each under load. The servo supply must be separate, with a common ground shared with the logic side.
