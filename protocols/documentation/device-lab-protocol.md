# Device Lab Documentation Protocol

Standard operating procedure for documenting computing devices intended for inventory tracking and hacking/tinkering projects.

---

## Table of Contents

1. [Overview](#overview)
2. [Directory Structure](#directory-structure)
3. [Device Categories](#device-categories)
4. [Interactive Workflow](#interactive-workflow)
5. [README Template](#readme-template)
6. [Hackability Documentation](#hackability-documentation)
7. [Download Checklist](#download-checklist)
8. [File Verification](#file-verification)
9. [Maintenance Tasks](#maintenance-tasks)

---

## Overview

This protocol covers consumer computing devices that don't fit into Programmable Devices or Vintage Computers categories. Focus is on:

- **Inventory tracking** - What you have, specs, condition
- **Hackability** - Custom firmware, root/jailbreak, homebrew potential
- **Project ideas** - Repurposing old devices for new uses

### Design Principles

1. **Hackability focus** - Document custom firmware options and modification potential
2. **Practical specs** - Focus on specs that matter for projects (storage, connectivity, I/O)
3. **Status tracking** - Active use vs. project candidate vs. parts donor
4. **Cross-reference** - Link to related projects in other repositories

---

## Directory Structure

```
Device Lab/
├── readme.md                      # Master index with all devices
├── mobile/
│   ├── samsung-galaxy-tab4-nook/
│   │   ├── README.md
│   │   └── [documentation files]
│   ├── amazon-fire-hd/
│   ├── apple-ipad-mini/
│   └── apple-iphone-2g/
├── gaming/
│   └── sony-psvita/
├── network-storage/
│   ├── wd-mycloud-pr4100/
│   └── wd-mybooklive/
├── desktops/
│   └── [desktop-name]/
├── laptops/
│   └── [laptop-name]/
├── networking/
│   └── [routers, switches, access points]
└── other/
    └── [anything that doesn't fit above]
```

### Directory Naming Convention

- Use lowercase with hyphens: `manufacturer-model`
- Include variant where significant: `apple-ipad-mini-1`, `amazon-fire-hd-8-2020`
- Keep names concise but identifiable

---

## Device Categories

| Category | Examples | Key Hackability Traits |
|----------|----------|----------------------|
| **mobile/** | Tablets, phones, e-readers | Root/jailbreak, custom ROM, kiosk mode |
| **gaming/** | Handhelds, consoles | CFW, homebrew, emulation |
| **network-storage/** | NAS, external drives | SSH access, custom apps, Docker |
| **desktops/** | Desktop PCs, workstations | Full control, parts, server conversion |
| **laptops/** | Laptops, netbooks, Chromebooks | Linux install, lightweight distros |
| **networking/** | Routers, switches, APs | OpenWrt, DD-WRT, custom firmware |
| **other/** | Media players, smart home, misc | Various - device dependent |

---

## Interactive Workflow

### Workflow: Adding New Devices

```
┌─────────────────────────────────────────────────────────────────┐
│  USER: "I want to add my [device] to the Device Lab"            │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  ASK: What is the exact model?                                  │
│       - Check Settings > About on the device                    │
│       - Model number on back/bottom                             │
│       - Generation or year if applicable                        │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  ASK: What is the current status?                               │
│       - Current OS/firmware version                             │
│       - Working condition (fully working, issues, parts only)   │
│       - Already rooted/jailbroken/modded?                       │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  RESEARCH: Look up specifications and hackability               │
│       - Official specs from manufacturer                        │
│       - XDA Developers, iFixit, Reddit for hackability          │
│       - Custom firmware availability                            │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  CREATE: Directory and README.md with specifications            │
│       - Use appropriate category subfolder                      │
│       - Document both specs AND hackability options             │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  DOWNLOAD: Relevant documentation                               │
│       - Service manuals, teardowns                              │
│       - Custom firmware guides                                  │
│       - Pinouts for any exposed headers                         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  UPDATE: Main readme.md index                                   │
└─────────────────────────────────────────────────────────────────┘
```

### Conversation Templates

#### Starting the conversation

```
I'd be happy to add that to your Device Lab. To document it properly, can you tell me:

1. What's the exact model? (check Settings > About or the label on the device)
2. What generation/year if there are multiple versions?
3. Current firmware/OS version?
4. Is it rooted/jailbroken/modded already?
5. What condition is it in? (daily use, shelved, project candidate, parts)
```

#### When researching hackability

```
I found these hackability options for the [Device]:

| Option | Status | Notes |
|--------|--------|-------|
| Root/Jailbreak | Available/Tethered/Unavailable | [method] |
| Custom ROM | [LineageOS/etc] | [versions supported] |
| Bootloader Unlock | Official/Unofficial/Locked | [how] |
| USB Debug | Available | [how to enable] |

Would you like me to document any specific modification paths?
```

---

## README Template

```markdown
# [Manufacturer] [Model]

**[Brief description - what it is and potential use]**

## Device Information

| Field | Value |
|-------|-------|
| Manufacturer | |
| Model | |
| Model Number | |
| Generation/Year | |
| Date Acquired | |
| Condition | Working / Issues / Parts |
| Current Use | Active / Shelved / Project |

## Specifications

| Specification | Value |
|--------------|-------|
| SoC/CPU | |
| RAM | |
| Storage | |
| Display | |
| Connectivity | WiFi, Bluetooth, USB, etc. |
| Ports | |
| Battery | |
| OS (Stock) | |
| OS (Current) | |

## Hackability

### Current Status

| Aspect | Status |
|--------|--------|
| Root/Jailbreak | Yes / No / Available |
| Bootloader | Locked / Unlocked / Unlockable |
| Custom Firmware | Installed / Available / None |
| ADB/SSH Access | Yes / No |

### Available Modifications

| Modification | Availability | Notes |
|-------------|--------------|-------|
| [Custom ROM] | [Version] | [Link/Status] |
| [Root Method] | [Tool] | [Requirements] |
| [Other Mod] | | |

### Hackability Resources

- [XDA Thread](url) - Main development thread
- [Custom Firmware](url) - Download page
- [Root Guide](url) - Step-by-step instructions

## Project Ideas

- [ ] Idea 1 - Brief description
- [ ] Idea 2 - Brief description
- [ ] Idea 3 - Brief description

## Documentation

### Official
- [Product Page](url)
- [Support Page](url)
- [Specs Sheet](url)

### Community
- [iFixit Teardown](url)
- [XDA Forum](url)
- [Reddit Community](url)

## Local Files

- `filename.pdf` - Description

## Notes

Any special notes about this device, known issues, modifications made, etc.
```

### Category-Specific Sections

**Mobile Devices** - Include: Screen size/resolution, cellular bands (if applicable), sensor list, stock apps removable

**Gaming Devices** - Include: Game format support, controller connectivity, homebrew store availability, emulation performance

**Network Storage** - Include: Drive bays, RAID support, network interfaces, CPU/RAM for transcoding, Docker/VM support

**Desktops/Laptops** - Include: Expansion slots, max RAM, storage interfaces, GPU, upgrade potential

**Networking** - Include: Wireless standards, port count/speed, flash size, RAM, OpenWrt support

---

## Hackability Documentation

### Hackability Rating System

Document hackability level for quick reference:

| Rating | Meaning | Examples |
|--------|---------|----------|
| ★★★★★ | Fully open, excellent support | Raspberry Pi, most Android devices |
| ★★★★☆ | Good support, some limitations | Older iOS devices, some tablets |
| ★★★☆☆ | Hackable with effort | Locked bootloaders, older firmware only |
| ★★☆☆☆ | Limited options | Newer locked devices, rare hardware |
| ★☆☆☆☆ | Minimal/no options | Heavily locked, no community support |

### Key Hackability Factors

| Factor | What to Document |
|--------|-----------------|
| **Bootloader** | Locked/unlockable/unlocked, official method available |
| **Root/Admin** | Method available, permanence (survives reboot/update) |
| **Custom Firmware** | Available ROMs/CFW, active development |
| **Recovery Mode** | Custom recovery available (TWRP, etc.) |
| **Debug Access** | ADB, SSH, serial console, JTAG |
| **Hardware Access** | Exposed headers, test points, expansion |

### Common Hackability Paths

#### Android Devices
- Bootloader unlock (OEM unlock or exploit)
- Custom recovery (TWRP)
- Root (Magisk)
- Custom ROM (LineageOS, etc.)

#### iOS Devices
- Jailbreak (checkra1n, unc0ver, etc.)
- Sideloading (AltStore)
- Note: Tied to specific iOS versions

#### Gaming Handhelds
- Custom firmware (CFW)
- Homebrew enabler (HEN)
- Memory card exploits

#### NAS Devices
- SSH access enablement
- Third-party package managers
- Full Linux/Docker access

#### Routers
- OpenWrt/DD-WRT compatibility
- Serial console access
- JTAG for recovery

---

## Download Checklist

### Documentation Priority

| Priority | Document Type | Source |
|----------|--------------|--------|
| 1 | Official Specs | Manufacturer website |
| 2 | Service Manual | iFixit, vendor (if available) |
| 3 | Teardown Guide | iFixit |
| 4 | Hackability Guide | XDA, Reddit, dedicated wikis |
| 5 | Pinout (if exposed headers) | Community sources |
| 6 | Custom Firmware | Project pages |

### Where to Find Documentation

| Device Type | Primary Sources |
|-------------|----------------|
| Android | XDA Developers, LineageOS wiki |
| iOS | iPhone Wiki, /r/jailbreak |
| Gaming | GBATemp, PSX-Place, /r/vitahacks |
| NAS | Manufacturer forums, /r/homelab |
| Routers | OpenWrt Table of Hardware |
| General | iFixit, manufacturer support |

---

## File Verification

### Verification Steps

```bash
# Check downloaded PDFs are valid
file *.pdf
# Expected: "PDF document, version X.X"

# Check file sizes
ls -lh *.pdf
# Manuals typically > 100KB

# Verify images
file *.png *.jpg 2>/dev/null
# Expected: "PNG image data" or "JPEG image data"
```

### Content Verification

- [ ] Document matches the exact model (not similar model)
- [ ] Content is in expected language
- [ ] Document is the right type (not marketing material)
- [ ] Guides match current firmware version where relevant

---

## Maintenance Tasks

### Periodic Review

| Task | Frequency | Description |
|------|-----------|-------------|
| Link check | Quarterly | Verify external URLs work |
| Hackability update | Quarterly | Check for new CFW/root methods |
| Status update | As needed | Update device condition/use status |
| Project ideas | As needed | Add new project ideas discovered |

### Quick Validation

```bash
# List devices without README
for dir in */*/; do
  if [ ! -f "$dir/README.md" ]; then
    echo "No README: $dir"
  fi
done
```

---

*Protocol version: 1.0*
*Created: January 2025*
