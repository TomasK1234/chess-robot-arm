# Software

PC-side software: board vision, game logic, move planning and the link to the arm.

**Status: not started.** This page describes the plan.

## Planned modules

| Module | Job |
|---|---|
| Vision | Capture frames from the overhead webcam, map pixels to board coordinates with a homography, and work out per-square occupancy |
| Move detection | Compare occupancy before and after the human's turn to find the move. Special cases: castling, en passant, promotion |
| Engine | Keep the game state and ask Stockfish for the robot's reply over UCI |
| Planner | Turn a move into a sequence of arm actions, including captures and the capture area |
| Comms | Send commands to the ESP8266 over WiFi and handle acknowledgements and errors |

## Planned tooling

- Python
- OpenCV for the vision pipeline
- A chess library for game state and move validation
- [Stockfish](https://stockfishchess.org/) as the engine

## Design choices

- **Occupancy, not piece recognition.** Telling empty squares from occupied ones is much more robust than classifying every piece type, and the game state already tells us which piece moved.
- **Thinking on the PC.** The microcontroller only executes motion; the engine and vision run where the compute is.
