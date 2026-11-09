# Changelog

All notable changes to this project are recorded here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Changed

- Layout resized to the measured chess set: 321 mm playing area, 77 mm king, measured grip heights (links 211.8 mm between axes)
- Baseboard size now includes the 29 mm board border
- Baseboard drawn 24 mm (`board_thickness`) below the playing surface, on the new plane `plane_baseboard_top`; new parameter `pedestal_height_above_baseboard` (112.7 mm)
- Power plan: one 5 V 6 A supply for servos and logic replaces the 6 V supply and logic buck converter
- Bill of materials updated with ordered parts, prices, chosen chess set (40 mm squares) and deferred items

### Added

- Project scope, goals and architecture overview in the README
- Design notes covering layout parameters, reach maths, precision, loads, bearings and the vertical stack
- Bill of materials with sources and status
- Build log
- Parametric Fusion 360 layout (`arm_layout`) with every dimension derived from `board_size`
- Placeholder CAD models for the MTR7 and MTR11 servos
- Reach verification screenshots (far corner, board centre, nearest squares)
- Folder structure and READMEs for `hardware`, `firmware` and `software`
- MIT licence and `.gitignore`

### Fixed

- Servo resolution estimate: the full 180° needs roughly 500 to 2500 µs, so one step is about 0.44°, not 0.88°
- Stale figures in the design plan and bill of materials (link lengths, baseboard size, order quantities, `board_size`)
- A CAD export entry that had landed in this file moved to the root `CHANGELOG.md`
