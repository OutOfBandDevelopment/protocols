# Equipment Documentation Protocol

Standard operating procedure for creating, maintaining, and expanding the test equipment inventory documentation.

---

## Table of Contents

1. [Overview](#overview)
2. [Interactive Workflow](#interactive-workflow)
3. [Directory Structure](#directory-structure)
4. [Adding New Equipment](#adding-new-equipment)
5. [Reviewing Existing Documentation](#reviewing-existing-documentation)
6. [Documentation Download Procedures](#documentation-download-procedures)
7. [README Template](#readme-template)
8. [Troubleshooting Downloads](#troubleshooting-downloads)
9. [Maintenance Tasks](#maintenance-tasks)

---

## Overview

This protocol ensures consistent documentation across all test equipment in the inventory. Each device gets its own directory containing:

- A standardized README.md with specifications and links
- Locally archived PDFs (user manuals, service manuals, datasheets)
- Software/firmware files where appropriate

### Key Files

| File | Purpose |
|------|---------|
| `readme.md` | Main index with equipment tables and navigation |
| `EQUIPMENT_DOCUMENTATION_PROTOCOL.md` | This protocol document |
| `MISSING_DOCUMENTATION.md` | Tracks downloads that failed/require manual action |
| `sync-docs.bat` | Windows script to sync to OneDrive |

### Design Principles

1. **Self-contained** - Each device directory should be usable standalone
2. **Offline-first** - Download and archive documentation locally when possible
3. **Consistent format** - Use the standard README template for all devices
4. **Linked index** - Main readme.md provides navigation to all devices

---

## Interactive Workflow

This section describes how to work with users to gather equipment information and create documentation.

### Workflow: Adding New Equipment

```
┌─────────────────────────────────────────────────────────────────┐
│  USER: "I want to add my [equipment] to the inventory"          │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  ASK: What is the manufacturer and model number?                │
│       (Check label on front, rear, or bottom of unit)           │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  ASK: Can you tell me the hardware/firmware revision?           │
│       - Check startup screen                                    │
│       - Look for "Rev", "Ver", "HW" on label                    │
│       - Serial number prefix (first 4-6 chars)                  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  RESEARCH: Look up specifications online                        │
│       - Manufacturer website                                    │
│       - Datasheet archives                                      │
│       - User manual PDFs                                        │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  CONFIRM: "I found these specs - does this match your unit?"    │
│       - List key specifications                                 │
│       - Note any revision-specific differences                  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  CREATE: Directory and README.md with specifications            │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  DOWNLOAD: PDFs and software (see Download Checklist below)     │
│       - User manual                                             │
│       - Service manual                                          │
│       - Datasheet                                               │
│       - Programming/SCPI reference                              │
│       - Firmware (if open source or updatable)                  │
│       - Software installers (if small/portable)                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  VERIFY: Check all PDFs are valid (not HTML error pages)        │
│       - file *.pdf should show "PDF document"                   │
│       - Files should be > 50KB typically                        │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  UPDATE: Main readme.md index and directory structure           │
└─────────────────────────────────────────────────────────────────┘
```

### Download Checklist

For each new device, attempt to download these files locally:

#### Documentation (Priority Order)

| Priority | Document Type | Typical Filename |
|----------|--------------|------------------|
| 1 | User Manual | `<Model>_UserManual.pdf` |
| 2 | Datasheet | `<Model>_Datasheet.pdf` |
| 3 | Service Manual | `<Model>_ServiceManual.pdf` |
| 4 | Programming/SCPI Guide | `<Model>_ProgrammingGuide.pdf` |
| 5 | Quick Start Guide | `<Model>_QuickStart.pdf` |
| 6 | Application Notes | `AN_<number>_<topic>.pdf` |

#### Software/Firmware

| Type | When to Download | Storage |
|------|------------------|---------|
| Firmware updates | If device has updatable firmware | `firmware/` subdirectory |
| Open-source firmware | If available (e.g., DSO Nano, Bus Pirate) | `firmware/` subdirectory |
| Small utilities (<50MB) | If portable/standalone | `software/` subdirectory |
| Large software suites | Don't download - link only | Document in README |
| Driver packages | If hard to find online | `drivers/` subdirectory |

#### Download Commands

```bash
# Standard PDF download
curl -L -o "<Model>_UserManual.pdf" "<url>"

# With user agent (if blocked)
curl -L --user-agent "Mozilla/5.0" -o "<Model>_UserManual.pdf" "<url>"

# Verify download succeeded
file *.pdf  # Should show "PDF document", not "HTML" or "XML"
ls -lh *.pdf  # Check file sizes are reasonable
```

#### What NOT to Download

- Large software installers (>50MB) - link only
- Proprietary software requiring license - link only
- Content requiring login/registration - note in README
- Malware/cracked software - never

### Conversation Templates

#### Starting the conversation

```
I'd be happy to add that to your equipment inventory. To make sure I get
the right documentation, can you tell me:

1. What's the exact model number? (usually on the front panel or label)
2. Is there a revision letter/number? (like "Rev B" or "v2.0")
3. When did you acquire it? (helps identify the hardware generation)
```

#### When user doesn't know the revision

```
No problem - let's figure out which revision you have. Can you check:

1. Turn it on - does the startup screen show a version number?
2. Look at the rear/bottom label - any text after the model number?
3. What are the first 4-6 characters of the serial number?

If you can send me a photo of the label, I can help identify it.
```

#### When user provides partial information

```
Thanks! I found that the [Model] was made in several versions:

- [Model]A (2005-2008): [key difference]
- [Model]B (2008-2012): [key difference]
- [Model]C (2012-present): [key difference]

Based on [what they told you], yours is likely the [version].
Does that sound right?
```

#### When specifications need verification

```
I found these specifications for the [Model Rev X]:

| Spec | Value |
|------|-------|
| ... | ... |

A few questions to verify:
1. Does your unit have [specific feature]?
2. What interfaces are on the back? (USB/GPIB/RS-232/LAN)
3. Any installed options or upgrades?
```

#### When documentation can't be found

```
I couldn't find downloadable documentation for the [Model]. Here are some options:

1. Do you have any original manuals or CDs that came with it?
2. I can create a basic entry with the specs we know, and you can add docs later
3. Would you like me to search archive.org or equipment forums?

What would you prefer?
```

### Handling Special Cases

#### Rebranded Equipment

Many instruments are sold under multiple brand names:

```
The [Brand] [Model] appears to be a rebrand of [Original Brand] [Original Model].
Documentation for either should work. Would you like me to:
1. Use the original manufacturer's documentation
2. Search for your specific brand's documentation
3. Include both in the entry
```

#### Discontinued Equipment

```
The [Model] was discontinued in [year]. Documentation may be harder to find.
I'll check:
1. Archive.org Wayback Machine
2. Equipment enthusiast forums
3. University lab archives

In the meantime, do you have any original documentation?
```

#### Chinese/Generic Clones

```
This appears to be a generic or clone instrument. These often:
- Have limited official documentation
- Use common chipsets with known specs
- Have community-created resources

I'll search for:
1. Reviews that measured actual specifications
2. Teardown articles showing internal components
3. User forums with operational guides
```

### Workflow: Reviewing Existing Documentation

```
┌─────────────────────────────────────────────────────────────────┐
│  USER: "Review my equipment documentation"                      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  CHECK: All directories have README.md files                    │
│         All listed PDFs exist and are valid                     │
│         External links are working                              │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  REPORT: Summary of findings                                    │
│         - Complete entries                                      │
│         - Entries missing documentation                         │
│         - Broken links found                                    │
│         - Entries needing more detail                           │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  ASK: Would you like me to:                                     │
│       1. Fix the broken links?                                  │
│       2. Download missing documentation?                        │
│       3. Fill in missing specifications?                        │
└─────────────────────────────────────────────────────────────────┘
```

---

## Directory Structure

```
/equipment-inventory/
├── readme.md                          # Main index with equipment tables
├── EQUIPMENT_DOCUMENTATION_PROTOCOL.md # This file
├── MISSING_DOCUMENTATION.md           # Tracker for failed/manual downloads
├── sync-docs.bat                      # OneDrive sync script (Windows)
├── <device-directory>/
│   ├── README.md                      # Device specifications and info
│   ├── <ManualName>_UserGuide.pdf     # User manual
│   ├── <ManualName>_ServiceManual.pdf # Service manual (if available)
│   ├── <ManualName>_Datasheet.pdf     # Datasheet (if available)
│   ├── firmware/                      # Firmware files (if applicable)
│   └── software/                      # Software installers (if applicable)
```

### Directory Naming Convention

Use lowercase with hyphens:
- `manufacturer-model` (e.g., `rigol-ds1102e`)
- Omit common prefixes like "HP/Agilent/Keysight" - use most recognizable (e.g., `hp-34401a`)
- For generic devices, use brand-model (e.g., `korad-ka3005p`)

---

## Adding New Equipment

### Step 1: Gather Information from User

**Interactive approach**: Work with the user to gather details progressively. Start with what they know, then ask clarifying questions.

#### Minimum Required Information

```
Equipment Addition Request (Minimum)
====================================
1. Manufacturer/Brand: _______________
2. Model Number: _______________
3. Equipment Type: (oscilloscope, multimeter, power supply, etc.)
```

#### Extended Information (for accuracy)

```
Equipment Details (Extended)
============================
1. Manufacturer/Brand: _______________
2. Model Number: _______________
3. Hardware Revision: _______________ (e.g., Rev A, v2, HW 1.1)
4. Firmware Version: _______________ (if applicable)
5. Serial Number Prefix: _______________ (helps identify manufacturing date/batch)
6. Date of Manufacture: _______________ (if known, often on label)
7. Date Acquired: _______________
8. Equipment Type: _______________
9. Country of Origin: _______________ (some models have regional variants)
```

#### Identifying Hardware Revisions

Many devices have multiple hardware revisions with different capabilities. Help the user identify their specific revision:

**Where to find revision information:**

1. **Label on rear/bottom** - Look for "Rev", "Version", "HW", or "PCB" followed by a number/letter
2. **Startup screen** - Many digital instruments display hardware/firmware version at boot
3. **About/System menu** - Check Settings → System → About or similar
4. **Serial number decode** - First digits often indicate manufacture date or revision
5. **Physical differences** - Different connectors, button layouts, or case designs

**Common revision indicators:**

| Format | Example | Meaning |
|--------|---------|---------|
| Letter suffix | DS1102E vs DS1102D | Different model variant |
| Number suffix | TDS2024B vs TDS2024C | Hardware revision |
| Dash number | HP 34401A-001 | Option or region code |
| Version number | v1.0, v2.0 | Major hardware revision |
| Year code | 2019 model | Manufacturing year |

**Questions to help identify revision:**

```
To ensure I find the correct documentation for your specific unit:

1. Is there a letter or number after the model number? (e.g., "E", "B", "v2")
2. What does the startup screen show for hardware/firmware version?
3. What is the serial number prefix? (first 4-6 characters)
4. Are there any option codes on the label? (e.g., "-001", "/OPT")
5. Approximately when was it manufactured or purchased?
```

**Why revisions matter:**

- Different hardware may have different specifications
- Firmware compatibility varies by hardware revision
- Service manuals may be revision-specific
- Some features added/removed between revisions
- Calibration procedures may differ

#### Follow-up Questions (ask as needed)

If the user doesn't provide full specifications, ask:

**For Oscilloscopes:**
- What is the bandwidth? (e.g., 100 MHz, 200 MHz)
- How many channels?
- Is it digital (DSO) or analog?
- What interfaces does it have? (USB, GPIB, RS-232, LAN)
- Do you have any documentation or know where to find it?

**For Multimeters:**
- What is the resolution? (3½, 4½, 5½, 6½ digit)
- Benchtop or handheld?
- What functions does it have? (voltage, current, resistance, capacitance, frequency, temperature)
- What interfaces? (USB, GPIB, RS-232)

**For Power Supplies:**
- What are the voltage and current ranges?
- How many channels?
- Is it programmable/remote controllable?
- Linear or switching?

**For Signal Generators:**
- What is the maximum frequency?
- How many channels?
- What waveforms? (sine, square, arbitrary, etc.)
- Modulation capabilities?

**For LCR Meters:**
- What test frequencies are available?
- Handheld or benchtop?
- What parameters can it measure? (L, C, R, D, Q, ESR)

**For Logic Analyzers:**
- How many channels?
- What is the maximum sample rate?
- Does it have analog channels too?
- What protocol decoders are supported?

**For Programmers:**
- What device types does it support? (EEPROM, Flash, MCU, GAL)
- What is the interface? (USB, parallel)
- What software does it use?

#### Research Assistance

If the user only provides manufacturer and model, offer to research:

```
I can look up the specifications for the [Model]. Would you like me to:
1. Search for specifications and documentation online
2. Create a basic entry that you can fill in later
3. Ask you specific questions about the device

Which approach would you prefer?
```

#### Specification Confirmation

After researching, confirm findings with the user:

```
I found the following specifications for [Model]:
- Bandwidth: 100 MHz
- Channels: 2
- Sample Rate: 1 GSa/s
- Memory: 1 Mpts

Does this match your unit? Any corrections or additions?
```

### Step 2: Create Directory

```bash
mkdir -p /path/to/inventory/<manufacturer-model>
```

### Step 3: Research Documentation

Search for documentation in this order:

1. **Manufacturer website** - Most authoritative
2. **Adafruit/SparkFun/Digikey** - Often host PDFs for products they sell
3. **ManualsLib.com** - Large manual archive
4. **Archive.org Wayback Machine** - For discontinued products
5. **University lab pages** - Often host equipment manuals
6. **GitHub repositories** - For open-source hardware

### Step 4: Create README.md

Use the [README Template](#readme-template) below.

### Step 5: Download Documentation and Software

**This step is mandatory** - always attempt to download documentation locally.

#### 5a. Download PDFs

```bash
cd /path/to/inventory/<manufacturer-model>

# Download user manual
curl -L -o "<Model>_UserManual.pdf" "<manual_url>"

# Download service manual (if available)
curl -L -o "<Model>_ServiceManual.pdf" "<service_manual_url>"

# Download datasheet (if available)
curl -L -o "<Model>_Datasheet.pdf" "<datasheet_url>"

# Download programming guide (if applicable)
curl -L -o "<Model>_ProgrammingGuide.pdf" "<programming_guide_url>"
```

#### 5b. Verify Downloads

**Step 1: Check file type**

```bash
# Check all PDFs are valid (not HTML error pages)
file *.pdf

# Expected output: "PDF document, version X.X"
# If you see "HTML document" or "XML", the download failed

# Check file sizes (PDFs should typically be > 100KB)
ls -lh *.pdf

# Remove any failed downloads
rm -f *.pdf  # only if file command shows HTML/XML
```

**Step 2: Verify PDF content matches the device**

This is critical - a valid PDF file may still be the wrong document!

```bash
# Extract first page text to verify content
pdftotext -l 1 "<filename>.pdf" - | head -20

# Or if pdftotext not available, check PDF metadata
pdfinfo "<filename>.pdf"
```

**Content verification checklist:**

- [ ] PDF title/header mentions the correct manufacturer
- [ ] PDF title/header mentions the correct model number
- [ ] Content appears to be the right document type (manual vs datasheet vs schematic)
- [ ] Language is appropriate (English or user's preferred language)
- [ ] Document is not for a completely different product

**Common issues:**

| Problem | Symptom | Solution |
|---------|---------|----------|
| Wrong product | PDF is for different model | Delete and find correct URL |
| Generic datasheet | Shows component, not full product | Search for actual user manual |
| Marketing brochure | No technical content | Search for technical documentation |
| Wrong language | Document in Chinese/German/etc | Search for English version |
| Corrupted PDF | Opens but content garbled | Re-download or try alternate source |

**If content verification fails:**

1. Delete the incorrect PDF immediately
2. Document the issue in the README
3. Search for alternate sources
4. If no correct documentation available, note in README:
   ```markdown
   ## Local Files

   *No official documentation found. [Reason/notes]*
   ```

#### 5c. Download Firmware (if applicable)

```bash
# Create firmware directory
mkdir -p firmware

# Download firmware files
curl -L -o "firmware/<firmware_file>" "<firmware_url>"
```

#### 5d. Handle Download Failures

If downloads fail:
1. Try adding `--user-agent "Mozilla/5.0"` to curl
2. Try archive.org: `https://web.archive.org/web/<year>/<original_url>`
3. Search for alternate sources (university sites, Adafruit, SparkFun)
4. If no source available:
   - Document in the device README:
     ```markdown
     ## Local Files

     *Note: Documentation requires manual download from [source] (registration required).*
     ```
   - **Add entry to `MISSING_DOCUMENTATION.md`** with:
     - Device name and document type
     - Best known download URL
     - Reason for failure (blocked, requires login, not found)
     - Alternative sources to try

#### 5e. Update README Local Files Section

After downloading, update the README to list local files:

```markdown
## Local Files

- `<Model>_UserManual.pdf` - User manual
- `<Model>_ServiceManual.pdf` - Service manual
- `<Model>_Datasheet.pdf` - Datasheet
```

### Step 6: Update Main Index

Add entry to the appropriate table in the main `readme.md`:

```markdown
| [Model Name](directory-name/) | Type | Key Spec | Other Spec | `directory-name/` |
```

---

## Reviewing Existing Documentation

### Periodic Review Checklist

For each device directory, verify:

- [ ] README.md exists and follows template
- [ ] All listed PDFs are present and valid (not HTML error pages)
- [ ] External links are still active
- [ ] Specifications are accurate
- [ ] Software links point to current versions

### Validation Commands

```bash
# Check all PDFs are valid
find . -name "*.pdf" -exec file {} \; | grep -v "PDF document"

# List directories without PDFs (may need manual download)
for dir in */; do
  if [ -z "$(find "$dir" -name '*.pdf' 2>/dev/null)" ]; then
    echo "No PDFs: $dir"
  fi
done

# Check for broken symlinks
find . -type l ! -exec test -e {} \; -print
```

### Link Verification

Test external documentation links periodically:

```bash
# Extract and test URLs from README files
grep -roh 'https\?://[^)]*' */README.md | sort -u | while read url; do
  status=$(curl -o /dev/null -s -w "%{http_code}" -L "$url" 2>/dev/null)
  if [ "$status" != "200" ]; then
    echo "BROKEN ($status): $url"
  fi
done
```

---

## Documentation Download Procedures

### Standard Download Command

```bash
curl -L -o "<output-filename>.pdf" "<url>"
```

### Download with User Agent (for sites that block curl)

```bash
curl -L --user-agent "Mozilla/5.0" -o "<output-filename>.pdf" "<url>"
```

### Verify Download Success

```bash
# Check file type (should say "PDF document")
file <filename>.pdf

# Check file size (PDFs are usually > 100KB)
ls -lh <filename>.pdf
```

### Common Download Issues

| Issue | Symptom | Solution |
|-------|---------|----------|
| Redirect to login | Small HTML file | Try archive.org or alternate source |
| Blocked by bot detection | 403 error | Add user-agent header |
| File moved | 404 error | Search for alternate URL |
| JavaScript required | HTML instead of PDF | Manual browser download |

### Fallback Sources

If primary source fails:

1. **Archive.org Wayback Machine**
   ```bash
   curl -L -o file.pdf "https://web.archive.org/web/2023/https://original-url.com/file.pdf"
   ```

2. **Search Engines**
   - `"<model number>" filetype:pdf user manual`
   - `"<model number>" filetype:pdf service manual`

3. **Document the failure** in README.md:
   ```markdown
   ## Local Files

   *Note: Documentation requires manual download from [source] (registration required).*
   ```

---

## README Template

```markdown
# [Manufacturer] [Model]

**[Equipment Type Description]**

## Unit Information

| Field | Value |
|-------|-------|
| Manufacturer | [Brand/Company] |
| Model | [Model Number] |
| Hardware Revision | [Rev/Version if known] |
| Firmware Version | [Version if applicable] |
| Serial Number | [Full or prefix, optional] |
| Date of Manufacture | [If known] |
| Date Acquired | [When you got it] |
| Condition | [Working/Needs Repair/For Parts] |

## Specifications

| Specification | Value |
|--------------|-------|
| [Primary Spec] | [Value] |
| [Secondary Spec] | [Value] |
| Channels/Ports | [Value] |
| Resolution | [Value] |
| Interfaces | [List] |

## Features

- Feature 1
- Feature 2
- Feature 3

## Documentation

- [User Manual (PDF)](https://example.com/manual.pdf)
- [Service Manual (PDF)](https://example.com/service.pdf)
- [Datasheet](https://example.com/datasheet.pdf)
- [Programming Manual](https://example.com/programming.pdf)

## Software

- **[Software Name]** - Description
- [Download Link](https://example.com/software)

## Notes

Any special notes about this specific unit, configurations, or quirks.

## Revision History

| Date | Change |
|------|--------|
| [Date added] | Initial entry |
| [Date] | [What changed] |

## Local Files

- `Model_UserManual.pdf` - User manual
- `Model_ServiceManual.pdf` - Service manual

*Or if files couldn't be downloaded:*

*Note: Documentation requires manual download from [source].*
```

### Minimal README Template (Quick Entry)

For when you need to add equipment quickly and fill in details later:

```markdown
# [Manufacturer] [Model]

**[Equipment Type]**

## Unit Information

| Field | Value |
|-------|-------|
| Manufacturer | [Brand] |
| Model | [Model] |
| Date Acquired | [Date] |

## Specifications

*To be documented*

## Documentation

*To be researched*

## Notes

[Any quick notes about the unit]
```

### Equipment-Type Specific Sections

**Oscilloscopes** - Include: bandwidth, channels, sample rate, memory depth, trigger types

**Multimeters** - Include: digit resolution, accuracy, measurement types, reading rate

**Power Supplies** - Include: voltage/current ranges, regulation specs, ripple, programmability

**Signal Generators** - Include: frequency range, waveform types, modulation capabilities

**Logic Analyzers** - Include: channel count, sample rate, protocol decoders

**LCR Meters** - Include: test frequencies, measurement ranges, accuracy

**Programmers** - Include: supported device types, interfaces, software options

---

## Troubleshooting Downloads

### PDF Downloads as HTML

```bash
# Check what was actually downloaded
file downloaded_file.pdf
head -20 downloaded_file.pdf

# If HTML, remove and try alternate source
rm downloaded_file.pdf
```

### SSL Certificate Errors

```bash
# Use -k flag (insecure) for self-signed certs
curl -k -L -o file.pdf "https://example.com/file.pdf"
```

### Large File Downloads

```bash
# Resume interrupted downloads
curl -C - -L -o file.pdf "https://example.com/file.pdf"

# Show progress
curl -L --progress-bar -o file.pdf "https://example.com/file.pdf"
```

### Rate Limiting

```bash
# Add delay between downloads
sleep 2 && curl -L -o file.pdf "https://example.com/file.pdf"
```

---

## Maintenance Tasks

### Adding New Equipment (Quick Reference)

1. `mkdir <manufacturer-model>`
2. Create `README.md` from template
3. Download PDFs with `curl -L -o`
4. Verify PDFs with `file *.pdf`
5. Update main `readme.md` index

### Periodic Maintenance

| Task | Frequency | Description |
|------|-----------|-------------|
| Link check | Monthly | Verify external URLs still work |
| PDF validation | Monthly | Ensure no corrupted files |
| Software updates | Quarterly | Check for new software versions |
| Spec verification | As needed | Confirm specs match actual equipment |
| Missing docs review | Monthly | Check `MISSING_DOCUMENTATION.md` for items to retry |

### Managing Missing Documentation

The `MISSING_DOCUMENTATION.md` file tracks downloads that couldn't be automated:

**When to add entries:**
- Download fails after trying all fallback methods
- Site requires login/registration
- Document not available online
- Downloaded file was wrong content

**Entry format:**
```markdown
### Device Name
- [ ] **Document Type** - Description
  - Source: [best URL]
  - Issue: [why it failed]
  - Manual download: [instructions]
```

**When you manually obtain a document:**
1. Place file in the correct device directory
2. Update the device README's "Local Files" section
3. Move the entry in `MISSING_DOCUMENTATION.md` to "Completed" section:
   ```markdown
   ### Device Name
   - [x] **Document Type** - Downloaded YYYY-MM-DD
     - Verified content matches device
     - File: `device-folder/filename.pdf`
   ```

### Archival Best Practices

1. **Keep original filenames** when they're descriptive
2. **Rename to consistent format** when originals are cryptic:
   - `<Model>_UserManual.pdf`
   - `<Model>_ServiceManual.pdf`
   - `<Model>_Datasheet.pdf`
3. **Document download date** in commit message or notes
4. **Preserve version numbers** in filenames for firmware

---

## Example: Adding a New Oscilloscope

```bash
# 1. Create directory
mkdir siglent-sds1104x-e

# 2. Download documentation
cd siglent-sds1104x-e
curl -L -o SDS1104X-E_UserManual.pdf "https://siglentna.com/USA_website_2014/Documents/UserManual/SDS1000X-E_UserManual.pdf"
curl -L -o SDS1104X-E_ServiceManual.pdf "https://siglentna.com/USA_website_2014/Documents/ServiceManual/SDS1000X-E_ServiceManual.pdf"
curl -L -o SDS1104X-E_ProgrammingGuide.pdf "https://siglentna.com/USA_website_2014/Documents/ProgrammingGuide/SDS1000X-E_ProgrammingGuide.pdf"

# 3. Verify downloads
file *.pdf

# 4. Create README.md (use template)

# 5. Update main readme.md with new entry in oscilloscopes table
```

---

## Request Templates

### User Request: Add New Equipment

```
I'd like to add the following equipment to the inventory:

Manufacturer: _______________
Model: _______________
Type: _______________

Key specs:
- _______________
- _______________

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
- [ ] Download additional manual
- [ ] Update software version
- [ ] Other: _______________

Details:
_______________
```

### User Request: Review Documentation

```
Please review documentation for: [ ] All equipment / [ ] Specific: _______________

Check for:
- [ ] Broken links
- [ ] Missing PDFs
- [ ] Outdated software versions
- [ ] Specification accuracy
```

---

*Protocol version: 1.0*
*Last updated: December 2024*
