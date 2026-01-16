# Project Review Protocol

Standard operating procedure for reviewing the repository inventory and updating `projects/project-ideas.md` and `workshop-capabilities.md` to reflect current capabilities.

---

## Table of Contents

1. [Overview](#overview)
2. [Interactive Workflow](#interactive-workflow)
3. [Inventory Review Checklist](#inventory-review-checklist)
4. [Cross-Reference Procedures](#cross-reference-procedures)
5. [Updating workshop-capabilities.md](#updating-workshop-capabilitiesmd)
6. [Updating projects/project-ideas.md](#updating-project-ideasmd)
7. [Project Idea Generation](#project-idea-generation)
8. [Verification Steps](#verification-steps)
9. [Maintenance Schedule](#maintenance-schedule)

---

## Overview

This protocol ensures that `projects/project-ideas.md` and `workshop-capabilities.md` accurately reflect the current inventory and capabilities documented in:

- `Programmable Devices/` - FPGAs, MCUs, development boards
- `Expansion Boards/` - Shields, HATs, capes, add-on modules
- `Test Equipment/` - Oscilloscopes, DMMs, programmers, tools
- `Vintage Computers/` - Apple II, TI-99/4A, and accessories
- `Camera Collection/` - Cameras, lenses, photography equipment
- `AnalogComputers/` - Research materials and analog computing resources

### Key Files

| File | Purpose |
|------|---------|
| `workshop-capabilities.md` | Equipment summary, project ideas by difficulty/category, troubleshooting guides, wishlist |
| `projects/project-ideas.md` | Personal project backlog with status notes |
| `links.md` | Reference links organized by topic |

### Design Principles

1. **Accurate reflection** - Both files should match current inventory
2. **Cross-referenced** - Projects should reference specific equipment
3. **Actionable** - Project ideas should be concrete and achievable with current equipment
4. **Skill-aware** - Projects calibrated to documented skills from GitHub repos
5. **No orphans** - Every major piece of equipment should enable at least one project

---

## Interactive Workflow

### Workflow: Full Project Review

```
┌─────────────────────────────────────────────────────────────────┐
│  TRIGGER: User requests project review or periodic maintenance  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  PHASE 1: INVENTORY AUDIT                                       │
│  Scan all inventory directories for current equipment           │
│  - Count devices in each category                               │
│  - Note any new additions since last review                     │
│  - Flag any removed/sold items                                  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  PHASE 2: CROSS-REFERENCE CHECK                                 │
│  Compare inventory against workshop-capabilities.md             │
│  - Equipment Summary tables                                     │
│  - "What Can I Build?" sections                                 │
│  - Project Ideas tables                                         │
│  - Troubleshooting guides                                       │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  PHASE 3: GAP ANALYSIS                                          │
│  Identify discrepancies                                         │
│  - Equipment in inventory but missing from capabilities         │
│  - Equipment in capabilities but not in inventory               │
│  - Projects referencing unavailable equipment                   │
│  - New combinations enabling new project types                  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  PHASE 4: PROJECT IDEA GENERATION                               │
│  Based on current inventory, suggest new projects               │
│  - Cross-platform projects (FPGA + vintage, etc.)               │
│  - Protocol bridge opportunities                                │
│  - Learning projects for underutilized equipment                │
│  - Projects building on existing GitHub work                    │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  PHASE 5: UPDATE FILES                                          │
│  Apply changes to both files                                    │
│  - Update equipment tables                                      │
│  - Add/remove project ideas                                     │
│  - Update cross-references                                      │
│  - Update "Last updated" timestamp                              │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  PHASE 6: VERIFICATION                                          │
│  Confirm changes are consistent                                 │
│  - All equipment referenced in projects exists                  │
│  - All major equipment has associated projects                  │
│  - Difficulty ratings are appropriate                           │
│  - GitHub repo links are current                                │
└─────────────────────────────────────────────────────────────────┘
```

### Workflow: Quick Sync (After Adding New Equipment)

```
┌─────────────────────────────────────────────────────────────────┐
│  TRIGGER: User just added new equipment to inventory            │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  IDENTIFY: What was added?                                      │
│  - Category (programmable device, test equipment, etc.)         │
│  - Key capabilities it provides                                 │
│  - Protocols/interfaces it supports                             │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  UPDATE: workshop-capabilities.md                               │
│  - Add to appropriate Equipment Summary table                   │
│  - Add to "What Can I Build?" if it enables new categories      │
│  - Add relevant project ideas by difficulty                     │
│  - Update troubleshooting if applicable                         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  SUGGEST: New project ideas for projects/project-ideas.md                │
│  - Projects specifically using the new equipment                │
│  - Projects combining new equipment with existing               │
└─────────────────────────────────────────────────────────────────┘
```

---

## Inventory Review Checklist

### Phase 1: Count and Catalog

Run these commands to get current inventory state:

```bash
# Count devices in each major category
echo "=== Programmable Devices ===" && ls -d "Programmable Devices"/*/ 2>/dev/null | wc -l
echo "=== Expansion Boards ===" && ls -d "Expansion Boards"/*/ 2>/dev/null | wc -l
echo "=== Test Equipment ===" && ls -d "Test Equipment"/*/ 2>/dev/null | wc -l
echo "=== Vintage Computers ===" && ls -d "Vintage Computers"/*/ 2>/dev/null | wc -l
echo "=== Camera Collection ===" && find "Camera Collection" -name "README.md" 2>/dev/null | wc -l
```

### Phase 2: Check for Missing READMEs

```bash
# Find directories without README.md
for dir in "Programmable Devices"/*/ "Expansion Boards"/*/ "Test Equipment"/*/; do
  if [ ! -f "$dir/README.md" ]; then
    echo "Missing README: $dir"
  fi
done
```

### Phase 3: Extract Key Capabilities

For each inventory section, identify:

| Category | Extract From README |
|----------|---------------------|
| Programmable Devices | MCU/FPGA type, memory, interfaces, programmers needed |
| Expansion Boards | Protocols supported, compatible platforms |
| Test Equipment | Measurement capabilities, frequency/resolution limits |
| Vintage Computers | CPU, expansion options, available peripherals |
| Camera Collection | Mounts, sensor specs, special capabilities |

---

## Cross-Reference Procedures

### Equipment Summary Tables

Compare inventory against these tables in `workshop-capabilities.md`:

| Table Section | Inventory Source |
|---------------|------------------|
| Compute Platforms | `Programmable Devices/` |
| Vintage Computers | `Vintage Computers/` |
| Test & Measurement | `Test Equipment/` |
| Assembly & Rework | `Test Equipment/` (soldering, hot air, etc.) |
| Protocol Tools | `Test Equipment/` (Bus Pirate, Analog Discovery, etc.) |
| Programmers | `Test Equipment/` (TL866, USBasp, etc.) |
| Fabrication | `Test Equipment/` (3D printer) |

### Project Cross-References

For each project in `workshop-capabilities.md`, verify:

1. **Platform exists** - The specified platform is in inventory
2. **Tools available** - Required programmers/debuggers are documented
3. **Prior work valid** - GitHub repo links work and contain relevant code
4. **Difficulty appropriate** - Based on documented skills

### GitHub Repository Check

Verify these repositories are still active and linked correctly:

| Repository | Purpose | Check |
|------------|---------|-------|
| [EmbeddedBakery](https://github.com/mwwhited/EmbeddedBakery) | Embedded projects | Active, has relevant code |
| [DeviceBridge](https://github.com/mwwhited/DeviceBridge) | Protocol bridges | Active, IEEE-1284 work |
| [MM8000](https://github.com/mwwhited-archives/MM8000) | 8085 emulator | Archived but accessible |
| [MyMashedDuet](https://github.com/mwwhited/MyMashedDuet) | 3D printer config | Active |
| [BinaryDataDecoders](https://github.com/mwwhited/BinaryDataDecoders) | Data encoding | Active |

---

## Updating workshop-capabilities.md

### Section Update Guidelines

#### Equipment Summary

When adding new equipment:

```markdown
| Category | Devices | Best For |
|----------|---------|----------|
| **[Category]** | [Device list] | [Primary use cases] |
```

- Use bold for category names
- List multiple devices comma-separated
- Keep "Best For" to 3-5 words

#### What Can I Build?

Format for new capability areas:

```markdown
### [Category Name]

*[Context note about prior work or equipment]*

- [ ] Project idea 1
- [ ] Project idea 2
```

- Include context linking to relevant GitHub repos
- Use checkbox format for trackable items
- Group related projects together

#### Project Ideas by Difficulty

| Difficulty | Criteria |
|------------|----------|
| **Quick Builds** | Hours to days, single platform, clear path |
| **Medium Projects** | Days to weeks, may combine platforms, some research needed |
| **Major Projects** | Weeks to months, significant design work, novel challenges |

Table format:

```markdown
| Project | Platform | Prior Work |
|---------|----------|------------|
| [Name] | [Platform + key components] | [GitHub link or skill reference] |
```

#### Troubleshooting Guide

Add new sections when equipment enables new repair capabilities:

```markdown
### "[Problem Category]"

\```
1. First diagnostic step
2. Second step
3. ...
\```
```

#### Wishlist

Format for wishlist items:

```markdown
- [ ] **[Equipment type]** - [Why needed, what it enables]
  - Budget/Used:
    - [Option 1 with price/link]
    - [Option 2]
  - New:
    - [Option 1 with link]
    - [Option 2]
```

### Timestamp Update

Always update the footer:

```markdown
*Last updated: [Month Year]*
```

---

## Updating projects/project-ideas.md

### Format Guidelines

Current format is a simple bullet list. Maintain this style:

```markdown
* [Project description] [Optional status in brackets]
```

### Status Indicators

| Indicator | Meaning |
|-----------|---------|
| No brackets | Not started |
| `[status note]` | In progress or has specific context |
| `[DONE]` | Completed (consider moving to archive) |
| `[BLOCKED: reason]` | Waiting on something |

### Adding New Ideas

When adding from inventory review:

1. **Be specific** - Name the exact platform/equipment
2. **Include motivation** - Why this project? What does it teach/produce?
3. **Reference prior work** - Link to existing code/skills when relevant

Example:

```markdown
* Build ROM dumper for TI-99/4A cartridges using Arduino Mega [have cartridge slot pinout documented]
```

### Removing/Archiving Ideas

Remove projects when:
- Equipment was sold/removed from inventory
- Project completed (add to a "completed" section or note)
- Interest has waned (be honest)

---

## Project Idea Generation

### Combination Matrix

When reviewing inventory, look for project opportunities by combining:

| Combine | With | Project Type |
|---------|------|--------------|
| FPGA | Vintage Computer | Soft core, peripheral clone |
| MCU | Test Equipment | Automation, data logging |
| Protocol Tool | Vintage Computer | Interface adapter |
| 3D Printer | Any electronics | Enclosures, fixtures |
| Analog Computer | MCU/FPGA | Hybrid computing |
| Camera | MCU | Remote trigger, data overlay |

### Skill-Building Projects

For underutilized equipment, suggest learning projects:

| Equipment Type | Learning Project |
|----------------|------------------|
| New FPGA board | Blinky → UART → VGA → CPU |
| New MCU | Blinky → I2C sensor → USB HID |
| New protocol tool | Decode existing device traffic |
| Analog computer | Start with integrator, build to chaos |

### GitHub Integration

New projects should consider:

1. **Extending existing repos** - Add to EmbeddedBakery, DeviceBridge, etc.
2. **New repo candidates** - Significant standalone projects
3. **Documentation value** - Projects that produce useful reference material

---

## Verification Steps

### Post-Update Checklist

After updating either file, verify:

- [ ] All equipment mentioned exists in inventory
- [ ] All GitHub links resolve correctly
- [ ] No duplicate projects across files
- [ ] Difficulty ratings are consistent
- [ ] New equipment has at least one associated project
- [ ] Timestamps are updated
- [ ] Markdown renders correctly (no broken tables)

### Consistency Checks

```bash
# Extract equipment names from workshop-capabilities.md
grep -oP '(?<=\| )[A-Za-z0-9\-]+ [A-Za-z0-9\-]+(?= \|)' workshop-capabilities.md | sort -u

# Compare against inventory directories
ls "Programmable Devices" "Test Equipment" "Expansion Boards" 2>/dev/null | sort -u
```

### Link Verification

For GitHub links, verify they return 200:

```bash
# Extract and test GitHub links
grep -oP 'https://github.com/[^\)]+' workshop-capabilities.md | while read url; do
  curl -s -o /dev/null -w "%{http_code} $url\n" "$url"
done
```

---

## Maintenance Schedule

### Recommended Review Frequency

| Trigger | Action |
|---------|--------|
| New equipment added | Quick Sync workflow |
| Equipment sold/removed | Remove from both files |
| Monthly | Quick inventory count check |
| Quarterly | Full Project Review workflow |
| After completing a project | Update status, add learnings |

### Review Prompts

Use these prompts to trigger a review:

**Full Review:**
> "Review the project inventory and update workshop-capabilities.md and projects/project-ideas.md"

**Quick Sync:**
> "I just added [equipment] to [category], sync the project files"

**Gap Analysis:**
> "What projects could I do with my current equipment that aren't listed?"

**Skill Check:**
> "Based on my GitHub repos, what projects am I ready for?"

---

## Documentation Protocols Maintenance

As part of the quarterly review, check that all documentation protocols are current and indexed in [projects/README.md](../projects/README.md).

### Protocol Currency Check

During Phase 5 (Update Files), verify protocol versions:

```bash
# Check all protocol versions
for f in .claude/protocols/*.md; do
  echo "=== $(basename $f) ===";
  grep -E "Protocol Version:|Last Updated:" "$f" | tail -2
done

# Document in projects/README.md with git hash for change tracking
git log -1 --format="%H %ad" .claude/protocols/PROJECTS_PROTOCOL.md
```

### Updating projects/README.md

The "Documentation Protocols Index" section in [projects/README.md](../projects/README.md) should be updated:

1. **Check protocol versions** in each `.claude/protocols/*.md` file
2. **Update version numbers** in the tables if any protocols changed
3. **Add git commit hash** (first 7 characters) for each protocol:
   ```bash
   git log -1 --format="%h" .claude/protocols/FILENAME.md
   ```
4. **Update last updated date** in projects/README.md footer
5. **Commit changes** with descriptive message: "Update protocol index in projects/README.md"

### Detecting Protocol Updates Between Reviews

To find which protocols changed since last review:

```bash
# If you know the review date:
git log --since="2026-01-01" --until="2026-04-01" \
  --format="%h %ad %s" .claude/protocols/ | grep -v "^commit"

# Or compare specific protocol:
git diff HEAD~20 .claude/protocols/PROJECTS_PROTOCOL.md | head -50
```

### When Protocols Need Updating

Update a protocol's version when:
- Adding new sections or guidance
- Clarifying existing instructions
- Changing recommendations or best practices
- Deprecating old patterns
- Integrating feedback from use

**DO NOT update version for:**
- Fixing typos (just commit the fix)
- Minor formatting changes
- Updating dates/links to external resources

### Protocol Version Format

Keep consistent version numbering:
- **1.0** = Initial release
- **1.1** = Minor updates (clarifications, new examples)
- **1.2** = Moderate updates (new sections, significant examples)
- **2.0** = Major changes (new workflow, breaking changes to recommendations)

### Including Protocol References

When documenting that a protocol was used or followed:

Include version and hash for auditability:
```markdown
## Following PROJECTS_PROTOCOL.md

Used version 1.1 (commit abc1234) created 2026-01-16

This project follows the standard structure documented in the current protocol:
- README.md with overview and materials
- build-log.md for session progress
- parts-list.md for detailed BOM
```

---

## Appendix: File Locations

| File | Path |
|------|------|
| This protocol | `.claude/protocols/PROJECT_REVIEW_PROTOCOL.md` |
| Workshop capabilities | `workshop-capabilities.md` |
| Project ideas | `projects/project-ideas.md` |
| Reference links | `links.md` |
| Programmable Devices index | `Programmable Devices/readme.md` |
| Expansion Boards index | `Expansion Boards/readme.md` |
| Test Equipment index | `Test Equipment/readme.md` |
| Vintage Computers index | `Vintage Computers/readme.md` |
| Camera Collection index | `Camera Collection/readme.md` |
| Analog Computing guide | `AnalogComputers/analog-computing-repository/CLAUDE.md` |

---

*Protocol Version: 1.1*
*Last Updated: 2026-01-16*

## Changelog

**v1.1 (2026-01-16)**
- Added section: "Documentation Protocols Maintenance"
- Added protocol currency check procedures
- Added guidance for updating projects/README.md with protocol versions
- Added git log commands for detecting protocol updates
- Added protocol version numbering guidelines
- Added template for including protocol references with version/hash
- Integrated protocol maintenance into quarterly review workflow

**v1.0 (December 2025)**
- Initial protocol specification
- Interactive workflow guidance
- Inventory review checklist
- Cross-reference procedures
- Gap analysis framework
- Project idea generation guidelines
