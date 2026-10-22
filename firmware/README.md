# Firmware

Firmware for the ESP8266 that receives commands over WiFi and drives the servos through a PCA9685 over I2C.

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
| I2C (SDA, SCL) | ESP8266 to the PCA9685 |
| Servo PWM | PCA9685 channels, one per servo |
| Servo power | Separate 6 V supply, with a common ground shared with the logic side |
| Logic power | Buck converter from the same supply |

## Planned tooling

- Arduino framework for the ESP8266
- Adafruit's PCA9685 servo driver library

The command protocol will be documented here once it is designed.
