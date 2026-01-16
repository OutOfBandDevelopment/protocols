# Expansion Boards Documentation Protocol

Standard operating procedure for creating, maintaining, and expanding the expansion boards inventory documentation.

---

## Table of Contents

1. [Overview](#overview)
2. [Interactive Workflow](#interactive-workflow)
3. [File Verification Procedures](#file-verification-procedures)
4. [Directory Structure](#directory-structure)
5. [Adding New Expansion Boards](#adding-new-expansion-boards)
6. [README Template](#readme-template)
7. [Platform-Specific Guidelines](#platform-specific-guidelines)
8. [Troubleshooting Downloads](#troubleshooting-downloads)
9. [Maintenance Tasks](#maintenance-tasks)
10. [Request Templates](#request-templates)

---

## Overview

This protocol ensures consistent documentation across all expansion boards in the inventory. Each board gets its own directory containing:

- A standardized README.md with specifications and links
- Locally archived PDFs (datasheets, schematics, user guides)
- **Datasheets for primary ICs on the board** (motor drivers, sensors, etc.)
- Pinout diagrams and pin mapping tables
- Example code or library links (where appropriate)

### Key Files

| File | Purpose |
|------|---------|
| `readme.md` | Main index with board tables and navigation |
| `EXPANSION_BOARDS_DOCUMENTATION_PROTOCOL.md` | This protocol document |
| `MISSING_DOCUMENTATION.md` | Tracks downloads that failed/require manual action |

### Design Principles

1. **Self-contained** - Each board directory should be usable standalone
2. **Offline-first** - Download and archive documentation locally when possible
3. **Consistent format** - Use the standard README template for all boards
4. **Linked index** - Main readme.md provides navigation to all boards
5. **Cross-referenced** - Link to compatible parent boards in `../Programmable Devices/`
6. **Pin conflict aware** - Document which pins are used to identify stacking conflicts

---

## Interactive Workflow

### Workflow: Adding New Expansion Boards

```
┌─────────────────────────────────────────────────────────────────┐
│  USER: "I want to add my [shield/HAT/cape] to the inventory"    │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  ASK: What is the manufacturer and model?                       │
│       What platform is it for? (Arduino, RPi, BeagleBone, etc.) │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  ASK: What is the primary function?                             │
│       - Motor control, sensors, display, communication, etc.    │
│       - What main ICs are on the board?                         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  RESEARCH: Look up specifications online                        │
│       - Manufacturer website                                    │
│       - IC datasheets                                           │
│       - Schematic PDFs                                          │
│       - Library/driver documentation                            │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  CONFIRM: "I found these specs - does this match your board?"   │
│       - List key specifications                                 │
│       - Confirm pin usage and compatibility                     │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  CREATE: Directory and README.md with specifications            │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  DOWNLOAD: PDFs and resources (see Download Checklist below)    │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  VERIFY: Check all files are valid and match board              │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  UPDATE: Main readme.md index                                   │
└─────────────────────────────────────────────────────────────────┘
```

### Download Checklist

#### Documentation (Priority Order)

| Priority | Document Type | Typical Filename |
|----------|--------------|------------------|
| 1 | Board Schematic | `<Board>_Schematic.pdf` |
| 2 | User Guide / Manual | `<Board>_UserGuide.pdf` |
| 3 | Main IC Datasheet | `<IC>_Datasheet.pdf` |
| 4 | Pinout Diagram | `<Board>_Pinout.png` |
| 5 | Getting Started Guide | `<Board>_GettingStarted.pdf` |
| 6 | Library Documentation | `<Board>_Library.pdf` |

#### Pin Mapping Requirements

**MANDATORY**: Every expansion board README must include:

1. **Pin Usage Table** - Which pins the board uses:
   ```markdown
   ## Pin Mapping

   | Board Pin | Arduino Pin | Function | Notes |
   |-----------|-------------|----------|-------|
   | D10 | 10 | CS | SPI chip select |
   | D11 | 11 | MOSI | SPI data |
   | D12 | 12 | MISO | SPI data |
   | D13 | 13 | SCK | SPI clock |
   ```

2. **Stacking Compatibility** - Can other shields be stacked?
   - Which pins are passed through
   - Which pins are consumed/blocked
   - Known conflicts with other shields

#### What NOT to Download

- Large IDE installers - link only
- Full library repositories - link to GitHub
- Content requiring login/registration - note in README

---

## File Verification Procedures

**CRITICAL**: All downloaded files MUST be verified.

### Verification Steps

```bash
# 1. Check file type is correct
file *.pdf *.png *.jpg 2>/dev/null

# Expected output:
# - PDFs: "PDF document, version X.X"
# - PNGs: "PNG image data, WxH, ..."

# 2. Check file sizes are reasonable
ls -lh *.pdf *.png *.jpg 2>/dev/null

# Minimum expected sizes:
# - User guides: > 50 KB
# - Schematics: > 20 KB
# - IC datasheets: > 100 KB

# 3. Detect HTML error pages disguised as PDFs
head -c 100 *.pdf | grep -l "<!DOCTYPE\|<html" 2>/dev/null
# If any output, those files are HTML, not PDF - delete and retry
```

---

## Directory Structure

```
/Expansion Boards/
├── readme.md                                       # Main index
├── EXPANSION_BOARDS_DOCUMENTATION_PROTOCOL.md      # This file
├── MISSING_DOCUMENTATION.md                        # Failed downloads
│
├── arduino-<shield-name>/                          # Arduino shields
│   ├── README.md
│   ├── <Board>_Schematic.pdf
│   ├── <IC>_Datasheet.pdf
│   └── <Board>_Pinout.png
│
├── rpi-<hat-name>/                                 # Raspberry Pi HATs
├── bbb-<cape-name>/                                # BeagleBone capes
├── esp-<shield-name>/                              # ESP shields
└── <platform>-<board-name>/                        # Other platforms
```

### Directory Naming Convention

Use lowercase with hyphens, prefixed by platform:
- Arduino shields: `arduino-motor-shield-r3`
- Raspberry Pi HATs: `rpi-sense-hat`
- BeagleBone capes: `bbb-relay-cape`
- ESP shields: `esp-oled-shield`
- Generic/multi-platform: `generic-<name>` or `<manufacturer>-<name>`

---

## Adding New Expansion Boards

### Step 1: Gather Information

#### Minimum Required Information

```
Expansion Board Addition Request (Minimum)
==========================================
1. Manufacturer/Brand: _______________
2. Board Name/Model: _______________
3. Platform: (Arduino/RPi/BeagleBone/ESP/Other)
4. Primary Function: _______________
5. Main ICs: _______________
```

### Step 2: Create Directory

```bash
mkdir -p "/path/to/Expansion Boards/<platform>-<board-name>"
```

### Step 3: Create README.md

Use the [README Template](#readme-template) below.

### Step 4: Download and Verify Documentation

```bash
cd "/path/to/Expansion Boards/<board-directory>"

# Download schematic
curl -L -o "<Board>_Schematic.pdf" "<schematic_url>"

# Download IC datasheet
curl -L -o "<IC>_Datasheet.pdf" "<datasheet_url>"

# Verify downloads
file *.pdf
ls -lh *.pdf
```

### Step 5: Update Main Index

Add entry to the appropriate table in the main `readme.md`.

---

## README Template

```markdown
# [Manufacturer] [Board Name]

**[Platform] [Board Type] - [Primary Function]**

## Board Information

| Field | Value |
|-------|-------|
| Manufacturer | [Brand/Company] |
| Board | [Board Name] |
| Platform | [Arduino/RPi/BeagleBone/ESP/etc.] |
| Form Factor | [Shield/HAT/Cape/etc.] |
| Primary Function | [Motor control/Sensors/Display/etc.] |
| Date Acquired | [When you got it] |
| Quantity | [How many you have] |

## Compatibility

| Compatible With | Notes |
|-----------------|-------|
| [Board 1] | [Link to ../Programmable Devices/board1/] |
| [Board 2] | [Any compatibility notes] |

## Specifications

| Specification | Value |
|--------------|-------|
| Main IC(s) | [Part numbers] |
| Operating Voltage | [Value] |
| Input Voltage | [Range] |
| Interfaces Used | [I2C, SPI, UART, GPIO, etc.] |
| Current Draw | [Value] |

## Pin Mapping

| Board Pin | Host Pin | Function | Notes |
|-----------|----------|----------|-------|
| [Pin] | [Pin] | [Function] | [Notes] |

### Pins Used

- **Power:** VIN, GND, 5V, 3.3V
- **Communication:** [List pins]
- **GPIO:** [List pins]

### Stacking Compatibility

- [ ] Passes through unused pins
- [ ] Can stack with: [list compatible shields]
- [ ] Conflicts with: [list incompatible shields]

## Features

- Feature 1
- Feature 2
- Feature 3

## Documentation

### Board Documentation
- [Schematic (PDF)](https://example.com/schematic.pdf)
- [User Guide](https://example.com/guide.pdf)
- [Product Page](https://example.com/product)

### IC Datasheets
- [Main IC Datasheet](https://example.com/ic-datasheet.pdf)

## Software & Libraries

### Official Libraries
- **[Library Name]** - [Description]
- [GitHub/Download Link](https://github.com/...)

### Installation
```
# Arduino Library Manager
Search for: "[Library Name]"

# Or manual installation
git clone https://github.com/...
```

## Example Code

```cpp
// Basic example
#include <Library.h>

void setup() {
    // Setup code
}

void loop() {
    // Main code
}
```

## Notes

Any special notes about this board, configurations, quirks, or modifications.

## Local Files

- `<Board>_Schematic.pdf` - Board schematic
- `<IC>_Datasheet.pdf` - Main IC datasheet
- `<Board>_Pinout.png` - Pinout diagram
```

---

## Platform-Specific Guidelines

### Arduino Shields

- Document Arduino pin numbers (D0-D13, A0-A5)
- Note 5V vs 3.3V compatibility
- Include library installation instructions
- Reference Arduino.cc documentation style

### Raspberry Pi HATs

- Document BCM GPIO numbers AND physical pin numbers
- Include HAT EEPROM information if applicable
- Note any device tree overlays required
- Reference pinout.xyz for standard pinouts

### BeagleBone Capes

- Document P8/P9 header pin assignments
- Include device tree overlay files if needed
- Note any pin mux requirements

### ESP Shields

- Note 3.3V logic level requirements
- Document any GPIO restrictions (strapping pins, etc.)
- Include both ESP8266 and ESP32 compatibility if applicable

---

## Troubleshooting Downloads

### Common Download Issues

| Issue | Symptom | Solution |
|-------|---------|----------|
| Redirect to login | Small HTML file | Try archive.org or alternate source |
| Blocked by bot detection | 403 error | Add user-agent header |
| File moved | 404 error | Search for alternate URL |
| JavaScript required | HTML instead of PDF | Manual browser download |

### If Downloads Fail

1. Try adding `--user-agent "Mozilla/5.0"` to curl
2. Try archive.org: `https://web.archive.org/web/<year>/<original_url>`
3. Search for alternate sources (Adafruit, SparkFun, GitHub)
4. Add entry to `MISSING_DOCUMENTATION.md`

---

## Maintenance Tasks

### Adding New Boards (Quick Reference)

1. `mkdir <platform>-<board-name>`
2. Create `README.md` from template
3. Download PDFs with `curl -L -o`
4. Verify PDFs with `file *.pdf`
5. Update main `readme.md` index

### Periodic Maintenance

| Task | Frequency | Description |
|------|-----------|-------------|
| Link check | Monthly | Verify external URLs still work |
| PDF validation | Monthly | Ensure no corrupted files |
| Library updates | Quarterly | Check for new library versions |
| Compatibility check | As needed | Verify with new parent board releases |

---

## Request Templates

### User Request: Add New Expansion Board

```
I'd like to add the following expansion board to the inventory:

Manufacturer: _______________
Board/Model: _______________
Platform: (Arduino/RPi/BeagleBone/ESP/Other)
Primary Function: _______________
Main ICs: _______________

Documentation links (if known):
- _______________

Notes:
_______________
```

### User Request: Update Existing Entry

```
Please update the entry for: _______________

Changes needed:
- [ ] Fix broken documentation link
- [ ] Add missing specification
- [ ] Update pin mapping
- [ ] Add library information
- [ ] Other: _______________

Details:
_______________
```

---

*Protocol version: 1.0*
*Last updated: December 2024*
