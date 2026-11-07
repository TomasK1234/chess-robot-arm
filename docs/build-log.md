# Build log

Newest entries first. Each entry records what was done and what was decided, with the reason.

## November 2026

### First parts order

- Ordered the Puecrof 15 inch folding chess set (40 mm squares, 3 inch king). The playing area becomes 320 mm, which keeps each link at about 291 mm overall, inside the Mark 2's 315 mm limit. A 45 mm-square board would have pushed the links past it.
- Ordered three MTR7 servos, two MTR11 servos (one spare for the gripper), two 3 inch lazy-susan bearings, a 1000 µF capacitor and a 2.1 mm DC socket.
- **Changed the power plan to a single 5 V 6 A supply** for both servos and logic. The planned 6 V 5 A supply cost £17.53 plus £24 delivery, a spare 12 V router adapter turned out to use a proprietary plug, and a 12 V supply plus a high-current buck cost about twice as much. Both servo types are rated from 4.8 V; the roughly 10 to 15 % torque loss is acceptable because the bearings carry the arm's weight. A 12 V supply with an XL4015 buck at 6 V is the fallback if the Z axis needs more torque.
- Deferred the Feather HUZZAH ESP8266, servo extension leads (lengths depend on cable routing), webcam, Z guides and grease. Dropped spare MTR7 and bearing.

## October 2026

### Layout, reach analysis and bearings

- Replaced the original articulated-arm plan with a **SCARA layout** after reviewing the alternatives, mainly because gravity no longer loads the main joints and the inverse kinematics become simple.
- Chose a **parallel gripper** with a real Staunton set, rather than a magnet.
- Built `arm_layout` in Fusion 360: a board sketch and a two-link arm skeleton, with every dimension driven by user parameters derived from `board_size`.
- Verified in the sketch that the arm reaches the far corner, the board centre and the nearest squares. The nearest squares need an elbow fold of about 160°, which makes clearance between the links the main thing to check in 3D.
- Worked out a first-pass vertical stack. The shoulder servo needs a pedestal, and the links need to sit high enough to carry a piece over a 76 mm king.
- Found that servo output shafts should not carry the arm's bending load, so **3 inch lazy-susan bearings** will carry the weight at the shoulder and elbow.
- Placed the shoulder servo reference in the layout, shaft on the shoulder axis and facing up.
- Settled the electronics direction: one 6 V supply for the servos, a buck converter for the logic rail, and WiFi so only a power cable reaches the arm.
- Created this repository.

## September 2026

### Scoping and parts

- Considered a drone, a desk-organising robot and a chess robot. Chose the **chess robot** for its bounded scope and clear demo, keeping the desk organiser as a possible second project on the same hardware.
- Dropped the gantry idea in favour of an arm that sits beside the monitor.
- Decided to keep the Adafruit PCA9685 Featherwing rather than move to an FPGA.
- Selected servos: three MTR7 metal-gear servos for the shoulder, elbow and Z axis, and one MTR11 mini servo for the gripper.
- Set up Fusion 360 user parameters and built placeholder models of both servos (`MTR7_reference`, `MTR11_reference`).
- Started `base_mount`, a plate around the shoulder servo.
