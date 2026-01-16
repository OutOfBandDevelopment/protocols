# Vintage Computers Documentation Protocol

This protocol guides documentation of vintage computer systems and peripherals in the `Vintage Computers/` directory.

## Overview

The Vintage Computers inventory tracks retro computing hardware including vintage computers (pre-2000s systems), peripherals, accessories, and modern peripherals that enable use of vintage systems.

## When to Use This Protocol

Use this protocol when:
- Adding a vintage computer system (Apple II, Commodore, TI-99/4A, etc.)
- Documenting vintage peripherals (disk drives, monitors, joysticks)
- Adding modern adapters/peripherals for vintage systems (Floppy Emu, HDMI adapters, RAM upgrades)
- Documenting restoration or upgrade projects
- Cross-referencing with FPGA soft cores or emulation projects

## What Qualifies as "Vintage"

**Include:**
- Pre-2000s home computers and workstations
- Peripherals designed for vintage systems
- Modern peripherals that enable vintage system use (Floppy Emu, VGA/HDMI adapters)
- Accessories specific to vintage platforms

**Exclude:**
- Modern computers → `Device Lab/` instead
- Test equipment → `Test Equipment/` instead
- Development boards → `Programmable Devices/` instead
- Modern general-purpose peripherals → `tools-and-components.md`

## Directory Structure

```
Vintage Computers/
├── readme.md                           # Main inventory index
├── apple-iic/
│   ├── README.md
│   ├── photos/
│   └── manuals/
├── apple-iigs/
│   ├── README.md
│   ├── photos/
│   └── manuals/
├── peripherals/
│   ├── floppy-emu/
│   │   ├── README.md
│   │   └── Floppy_Emu_Manual.pdf
│   ├── a2heaven-vga-adapter/
│   └── ramworks-compatible-4mb/
└── projects/
    ├── iigs-rom-upgrade/
    └── iic-composite-video-mod/
```

## Main readme.md Format

The top-level `readme.md` should contain:

```markdown
# Vintage Computers Inventory

Personal vintage computer collection including systems, peripherals, and modern adapters.

---

## Systems

| System | Year | CPU | RAM | Storage | Status | Directory |
|--------|------|-----|-----|---------|--------|-----------|
| [Apple IIc](apple-iic/) | 1984 | 65C02 | 128KB | Floppy Emu | Working | `apple-iic/` |
| [Apple IIgs](apple-iigs/) | 1986 | 65C816 | 4MB | Floppy Emu | Working | `apple-iigs/` |
| [TI-99/4A](ti-99-4a/) | 1981 | TMS9900 | 48KB | Cassette | Working | `ti-99-4a/` |

---

## Peripherals & Accessories

### Storage

| Device | Compatibility | Description | Directory |
|--------|---------------|-------------|-----------|
| [Floppy Emu Model C](peripherals/floppy-emu/) | Apple II, Mac | SD card floppy emulator | `peripherals/floppy-emu/` |

### Video Output

| Device | Compatibility | Description | Directory |
|--------|---------------|-------------|-----------|
| [A2Heaven VGA Adapter](peripherals/a2heaven-vga-adapter/) | Apple IIgs | DVI/VGA output adapter | `peripherals/a2heaven-vga-adapter/` |

### Memory Expansions

| Device | Compatibility | Description | Directory |
|--------|---------------|-------------|-----------|
| [RAMWorks Compatible 4MB](peripherals/ramworks-compatible-4mb/) | Apple IIe/IIgs | 4MB RAM expansion | `peripherals/ramworks-compatible-4mb/` |

---

## Collection Value Summary

| Category | Items | Total Value |
|----------|-------|-------------|
| Systems | 3 | $XXX.XX |
| Peripherals | 5 | $XXX.XX |
| **Total** | **8** | **$XXX.XX** |

---

## Cross-References

- [Programmable Devices/](../Programmable%20Devices/) - FPGA boards for retro core projects
- [projects/project-ideas.md](../projects/project-ideas.md) - 6502/65C816/TMS9900 soft core projects
- [EmbeddedBakery](https://github.com/mwwhited/EmbeddedBakery) - FPGA retro computing projects

---

*Last updated: Month YYYY*
```

## Individual System README Format

Each system should have a README.md:

```markdown
# System Name (Year)

Brief description of the system and its significance.

## Specifications

| Component | Details |
|-----------|---------|
| **CPU** | Processor type and speed |
| **RAM** | Memory configuration |
| **ROM** | ROM version/size |
| **Video** | Video capabilities |
| **Audio** | Audio capabilities |
| **I/O Ports** | Available ports |
| **Storage** | Connected storage devices |
| **Power** | Power supply specs |

## Serial Number

**Serial:** [Serial number if available]

## Condition

**Status:** [Fully Working / Partially Working / Needs Repair]

**Issues:**
- Any known problems
- Missing parts
- Cosmetic damage

## Purchase Information

| Source | Date | Price | Notes |
|--------|------|-------|-------|
| eBay/Seller | YYYY-MM-DD | $XXX.XX | Condition at purchase |

## Connected Peripherals

- [Floppy Emu Model C](../peripherals/floppy-emu/) - Boot disk and storage
- [A2Heaven VGA Adapter](../peripherals/a2heaven-vga-adapter/) - Modern display output
- ...

## Installed Software

- ProDOS 2.4.2
- GS/OS 6.0.1
- AppleWorks
- ...

## Modifications & Upgrades

### RAM Upgrade
- **Date:** YYYY-MM-DD
- **Upgrade:** 256KB → 4MB RAMWorks compatible
- **Source:** Vendor name
- **Cost:** $XXX.XX
- **Documentation:** [Link to upgrade docs](path)

### ROM Upgrade
- ...

## Documentation

- [Original Manual PDF](manuals/System_Manual.pdf)
- [Schematic PDF](manuals/System_Schematic.pdf)
- Online resources:
  - [Apple II Documentation Project](https://mirrors.apple2.org.za/)
  - [WikiPedia article](url)

## Photos

![System front view](photos/system-front.jpg)
![System rear ports](photos/system-rear.jpg)

## Projects & Experiments

- [IIgs ROM Upgrade](../projects/iigs-rom-upgrade/) - FPGA-based ROM replacement
- Software preservation with Greaseweazle
- Cross-reference to [projects/project-ideas.md](../../projects/project-ideas.md)

## Cross-References

### Related Equipment
- [Greaseweazle V4.1](../../Test%20Equipment/greaseweazle-v41/) - Floppy disk archival
- [Multicomp EP2C5-DB](../../Programmable%20Devices/multicomp-ep2c5db/) - 6502/65C816 soft cores

### FPGA Soft Cores
- 65C816 soft core project (Apple IIgs compatible)
- 6502 soft core on Arty A7

---

*Last updated: Month YYYY*
```

## Peripheral README Format

For peripherals and accessories:

```markdown
# Peripheral Name

Brief description of what it does and which systems it works with.

## Compatibility

- System 1 (specific model/version requirements)
- System 2
- ...

## Specifications

| Feature | Details |
|---------|---------|
| **Connection** | Port type (DB-19, DB-25, etc.) |
| **Power** | Power requirements |
| **Firmware** | Current firmware version |
| **Media** | Supported media types |

## Setup & Configuration

1. Connection steps
2. Configuration procedure
3. Firmware updates (if applicable)

## Usage

- How to use the peripheral
- Common operations
- Tips and tricks

## Purchase Information

| Source | Date | Price | SKU/Model |
|--------|------|-------|-----------|
| BMOW | 2023-XX-XX | $XXX.XX | Model-XX |

## Documentation

- [User Manual PDF](Peripheral_Manual.pdf)
- [Firmware Downloads](url)
- [Support Forum](url)

## Photos

![Device front](photos/device-front.jpg)

## Notes

- Compatibility notes
- Known issues
- Workarounds

---

*Last updated: Month YYYY*
```

## Categories

### Computer Systems
- Home computers (Apple II, Commodore, Atari, etc.)
- Workstations (Sun, SGI, NeXT, etc.)
- Portables (laptops, luggables)

### Peripherals - Storage
- Disk drives (floppy, hard drive)
- Tape drives
- Modern storage emulators (Floppy Emu, FlashFloppy, etc.)

### Peripherals - Video
- Monitors (CRT, LCD with appropriate inputs)
- Video adapters (VGA, HDMI converters)
- Graphics cards/upgrades

### Peripherals - Audio
- Sound cards
- Speech synthesizers
- MIDI interfaces

### Peripherals - Input
- Joysticks
- Mice
- Trackballs
- Graphics tablets

### Peripherals - Expansion
- RAM expansions
- ROM upgrades
- Interface cards

### Peripherals - Networking
- Modems
- Ethernet adapters
- Serial/parallel adapters

## Cross-References

### To Programmable Devices
When documenting FPGA soft cores:
```markdown
## Related Projects

- [Arty A7-100](../../Programmable%20Devices/digilent-arty-a7-100/) - Running 6502 soft core
- [Multicomp EP2C5-DB](../../Programmable%20Devices/multicomp-ep2c5db/) - Z80/6502/6809 retro computing kit
```

### To Test Equipment
When documenting tools used with vintage systems:
```markdown
## Archival Equipment

- [Greaseweazle V4.1](../../Test%20Equipment/greaseweazle-v41/) - For disk archival
```

### To projects/project-ideas.md
Link to planned projects:
```markdown
## Future Projects

See [projects/project-ideas.md](../../projects/project-ideas.md):
- 65C816 soft core (Apple IIgs compatible)
- TMS9900 soft core (TI-99/4A compatible)
```

### From projects/project-ideas.md
Reference vintage hardware:
```markdown
* Build 65C816 soft core → [Apple IIgs](Vintage%20Computers/apple-iigs/)
```

## Documentation Standards

### File Naming
- System directories: lowercase with hyphens (`apple-iic`, `ti-99-4a`)
- PDFs: `System_Model_Manual.pdf`, `Peripheral_Name_Guide.pdf`
- Photos: `YYYY-MM-DD-description.jpg` or `system-view.jpg`

### Photo Guidelines
- Take photos from multiple angles
- Include close-ups of ports/labels
- Document serial numbers
- Show condition issues

### Manual Archival
- Download official manuals when available
- Archive third-party documentation
- Include schematics if available
- Note scan quality/completeness

## Purchase Tracking

Track where systems came from:
```markdown
| Source | Date | Price | Condition | Notes |
|--------|------|-------|-----------|-------|
| eBay seller | 2023-05-15 | $125.00 | Working, yellowed case | Included power supply |
```

Cross-reference with ShoppingOnline.md if documenting seller experience.

## Restoration Projects

If restoring a system:
1. Document "before" state with photos
2. Track replacement parts and sources
3. Note modifications made
4. Document "after" state
5. Link to project in `projects/` if extensive

## Software Preservation

Document any software archival:
```markdown
## Disk Images Archived

- Original ProDOS boot disk (imaged with Greaseweazle)
- System software collection
- User data backup

**Storage Location:** `~/vintage-software/apple-iic/`
```

## Common Mistakes to Avoid

❌ **Don't:**
- Add modern PCs (use Device Lab instead)
- Add test equipment (use Test Equipment instead)
- Forget to document firmware versions
- Mix up system directories and peripheral directories

✅ **Do:**
- Cross-reference related equipment
- Document modifications and upgrades
- Include purchase information
- Link to FPGA soft core projects
- Archive manuals and schematics

---

*Protocol Version: 1.0*
*Last Updated: January 2026*
