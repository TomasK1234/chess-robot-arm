# Changelog

## 2026-10-22 10:19:32 - arm_layout
- **Branch:** `main`
- **Commit Message:** "Updated design"
- **Files Updated:**
  - `hardware\cad\arm_layout.f3d`
  - `hardware\cad\arm_layout.step`
  - `hardware\cad\arm_layout.stl`

---

All notable changes to this project are recorded here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Changed

- Layout resized to the measured chess set: 321 mm playing area, 77 mm king, measured grip heights (links 211.8 mm between axes)
- Baseboard size now includes the 29 mm board border
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
