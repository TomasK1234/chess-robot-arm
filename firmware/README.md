# Firmware

Firmware for the Raspberry Pi Pico W that receives commands over WiFi and drives the servos through a PCA9685 over I2C.

**Status: not started.** This page describes the plan.

## Planned responsibilities

- Drive the four servos through the Adafruit PCA9685 over I2C
- Move servos through interpolated waypoints rather than jumping to a target, so the arm moves smoothly and does not knock neighbouring pieces
- Hold the calibration data: servo counts for each of the 64 squares, the capture area and the home position
- Approach every target from the same rotational direction to take up backlash
- Enforce per-joint angle limits and move to a safe home position on power-up
- Accept commands over WiFi from the PC, such as "pick up from e2, place on e4"

## Planned hardware interface

| Signal | Connection |
|---|---|
| I2C (SDA, SCL) | Pico to the PCA9685 Featherwing by jumper wires; expected address 0x45, to confirm with a scan |
| Servo PWM | PCA9685 channels, one per servo |
| Servo power | 5 V 6 A supply into the Featherwing's servo terminal, with a 1000 µF capacitor across it |
| Logic power | Featherwing servo terminal, through a diode, into the Pico's VSYS pin. The Pico's 3V3(OUT) pin powers the Featherwing's logic |
| Ground | Pico GND to Featherwing GND |

With only USB connected, the Pico and the PCA9685 chip are powered and the servos are not. This is the safe state for uploading code.

## Planned tooling

- MicroPython on the Pico, chosen for its interactive prompt (REPL): servo positions can be typed in and tried immediately while calibrating
- A small PCA9685 driver written over `machine.I2C`

The command protocol will be documented here once it is designed.
