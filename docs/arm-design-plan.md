# Arm design plan

The staged plan for the mechanical design. Each stage ends with a check that must pass before moving on, and an export through the Fusion-to-GitHub add-in. Numbers are design targets until the real parts are measured.

## Approach

- **One master design.** `arm_layout` holds the user parameters, so every printed part becomes a component inside it, driven by those parameters. The servo and bearing models are inserted as linked reference components. The work in `base_mount` gets folded into the pedestal component. One document means one set of parameters and one place to check clearances.
- **Design from the gripper back to the base.** Each stage carries everything after it. The gripper and Z axis set the height of link 2, which sets link 1, which sets the pedestal height. That is why the pedestal is paused until the gripper and Z axis exist, even though work on it had started.
- **Shared baseboard.** The arm and the chess board mount on one baseboard (plywood or MDF). The arm cannot tip over (with roughly 330 g reaching about 300 mm out, a free-standing base would need about 1 kg of ballast). The board also stays registered to the arm, so square calibration and camera calibration survive being bumped.
- **Test the riskiest part first.** Before printing the whole arm, print one joint (bearing, servo and hub) and bench-test it with the servo driver.

## Stage 0: buy the reference parts (now)

Hole spacing, horn shape, shaft splines and bearing hole patterns aren't in the listings. Designing mounts around guesses means reprinting. These parts are cheap, so buy them now and measure them.

| Part | Qty | Why now |
|---|---|---|
| MTR7 servo (MG995 class) | 3 + 1 spare | Mounts, horns and body size for the shoulder, elbow and Z axis. Clones vary, so keep a spare. |
| MTR11 mini servo (SG90 class) | 1 + 1 spare | Gripper drive |
| 3 inch lazy-susan bearing | 3 | Hole pattern, plate thickness and actual play |
| 350 mm board and Staunton set | 1 | King height, neck heights and diameters drive the gripper and the whole vertical stack |

Measure with calipers: servo body length (without tabs), tab-to-tab length, mounting hole spacing and diameter, the height from the tab face to the shaft top, the horn hole pattern, and the shaft offset from the body centre. On the bearing: the hole positions on both plates, and how much it tilts under a light hand load. For each piece type: total height, neck height, the smallest neck diameter, and the widest part above the neck.

## Stage 1: upgrade the master layout

- Add parameters for the piece dimensions, the vertical stack heights, the bearing size and the gripper envelope.
- Put construction axes at the shoulder, elbow and wrist, and add work planes at each link height.
- **Check:** changing `board_size` still rebuilds everything.

## Stage 2: gripper

- **Concept:** a parallel gripper. The MTR11 turns a centre pinion that drives two opposed racks, so both jaws move together and stay centred on the piece. V-notched jaw pads self-centre on round necks.
- **Requirements:** the opening clears the widest piece head with margin. The open jaws fit between neighbouring pieces at grip height (squares are 43.75 mm). The grip lands on the neck of every piece type, including the knight.
- **Check:** in CAD, the open jaws clear the neighbouring pieces on a full board. Then print and test on real pieces.

## Stage 3: Z axis

- **Concept:** the servo turns a pinion driving a vertical rack on the gripper carriage. Two steel guide rods with printed or bronze bushings guide the carriage.
- **Travel:** from the grip height down at the board, up to where a carried piece's base clears the king by `lift_height` plus margin.
- **Decision for this stage:** keep an MTR7 (one servo type, already chosen) or use a lighter metal-gear micro servo (about 13 g instead of about 55 g at the tip). The bearings take the weight, so this mostly affects droop and inertia, not servo torque.
- **Check:** at the bottom of the travel, the gripper reaches grip height on the shortest piece (a pawn). At the top, the carried piece clears the king.

## Stage 4: forearm (link 2) and elbow

- **How the links are made depends on your printer bed.** Each link is about 231 mm between axes, plus the hubs, so roughly 300 mm overall. That is longer than most printer beds. Options:
  1. Printed hubs joined by aluminium square tube cut to length. Stiff and light, and the length stays a single parameter.
  2. A fully printed link split into two parts and bolted at the middle.
  3. A fully printed link printed diagonally, if the bed allows.
- **Elbow:** the servo mounts on the end of link 1 with its shaft up. Link 2 rides on the second bearing. The servo horn drives link 2 through a coupling with slight radial float, so the servo shaft and the bearing don't fight over the axis.
- **Check:** link 2 and the Z carriage clear link 1 and the shoulder hub at the full 160° fold.

## Stage 5: link 1 and shoulder hub

- The same construction as link 2. The shoulder hub couples the shoulder servo horn to the link and the bearing's rotating plate.
- **Check:** the full 4° to 130° shoulder sweep has no collisions.

## Stage 6: pedestal, base and electronics bay

- The pedestal height now follows from the stack. The shoulder servo hangs under the top plate.
- On the board side, the structure stays within about 54 mm of the shoulder axis, so the gripper can reach the nearest squares.
- An electronics bay in the base holds the ESP8266, the PCA9685, the buck converter, the barrel jack and the capacitor. Plan cable routes up the arm with slack loops at each joint.
- **Check:** fasteners can be reached with tools, and wires don't snag across the full joint ranges.

## Stage 7: motion check in CAD

- Add revolute joints at the shoulder and elbow and a slider for Z. Drive the arm to the far corner, the board centre, the nearest squares and the capture area.
- **Check:** no interference anywhere. The clearance zones from the design notes stay clear.

## Stage 8: prototype

1. Print the shoulder joint and bench-test it through its range.
2. Print the remaining parts and assemble.
3. Measure the real play at the tip and compare it with the precision estimates in the design notes.
