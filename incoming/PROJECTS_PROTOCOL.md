# Projects Documentation Protocol

This protocol guides documentation of active projects in the `projects/` directory.

## When to Use This Protocol

Use this protocol when:
- Creating documentation for a new project build
- Documenting an active project that spans multiple sessions
- Moving a project idea from `projects/project-ideas.md` to active implementation
- Creating build logs, parts lists, or progress tracking for a project

## Directory Structure

```
projects/
├── project-name/
│   ├── README.md              # Main project documentation
│   ├── build-log.md           # Optional: Step-by-step build progress
│   ├── parts-list.md          # Optional: BOM with sources and prices
│   ├── photos/                # Optional: Project photos
│   ├── schematics/            # Optional: Circuit diagrams, PCB files
│   └── code/                  # Optional: Firmware, software
```

## README.md Template

Each project should have a README.md with the following sections:

```markdown
# Project Name

Brief one-sentence description of what the project is.

## Overview

2-3 paragraphs describing:
- What the project does
- Why you're building it
- Key features or goals

## Materials

Link to materials in tools-and-components.md or create a parts-list.md

| Material | Purpose | Source | Purchased | Price |
|----------|---------|--------|-----------|-------|
| ... | ... | ... | ... | ... |

**Total materials cost:** $XXX.XX

*Compare to commercial alternative: $XXX.XX (if applicable)*

## Construction Method / Build Steps

High-level steps:
1. Step one
2. Step two
3. ...

*See [build-log.md](build-log.md) for detailed progress notes.*

## Key Features

- Feature 1
- Feature 2
- Feature 3

## Advantages Over Commercial/Alternative Solutions

| Feature | This Project | Alternative |
|---------|-------------|-------------|
| ... | ... | ... |

## Use Cases

- Use case 1
- Use case 2
- Use case 3

## Project Status

**Status:** [Planning / Materials Acquired / In Progress / Completed / On Hold]

**Next Steps:**
1. Next task
2. Following task
3. ...

## References

- [Link to inspiration](url)
- [Related documentation](path)
- Cross-reference to projects/project-ideas.md

---

*Last updated: YYYY-MM-DD*
```

## Build Log Format (Optional)

If the project warrants detailed progress tracking, create `build-log.md`:

```markdown
# Project Name - Build Log

## YYYY-MM-DD - Session Title

**Goals:**
- Goal 1
- Goal 2

**Progress:**
- Completed task 1
- Completed task 2
- Issue encountered: description

**Photos:**
![Description](photos/YYYY-MM-DD-photo1.jpg)

**Next Session:**
- Task for next time
- Questions to resolve

---

## YYYY-MM-DD - Previous Session

...
```

## Parts List Format (Optional)

For complex projects, create `parts-list.md`:

```markdown
# Project Name - Parts List (BOM)

## Electronics

| Part | Description | Qty | Source | Part # | Unit Price | Total |
|------|-------------|-----|--------|--------|------------|-------|
| ... | ... | ... | ... | ... | ... | ... |

## Hardware

| Part | Description | Qty | Source | Part # | Unit Price | Total |
|------|-------------|-----|--------|--------|------------|-------|
| ... | ... | ... | ... | ... | ... | ... |

## Materials

| Part | Description | Qty | Source | Part # | Unit Price | Total |
|------|-------------|-----|--------|--------|------------|-------|
| ... | ... | ... | ... | ... | ... | ... |

**Project Total:** $XXX.XX

## On Hand vs. Need to Purchase

- ✓ Already have: Part 1, Part 2
- ⚠ Need to buy: Part 3, Part 4
```

## Cross-References

### To tools-and-components.md
When materials are sourced from existing inventory:
```markdown
*Materials documented in [tools-and-components.md](../../tools-and-components.md#section-name)*
```

### From projects/project-ideas.md
When moving a project from ideas to active:
```markdown
**Related:** Originally documented in [projects/project-ideas.md](../../projects/project-ideas.md)
```

Update projects/project-ideas.md status:
```markdown
* ~~Build DIY TrekPak dividers~~ - **ACTIVE** [see projects/diy-trekpak-dividers/](projects/diy-trekpak-dividers/)
```

### To/From equipment inventories
Cross-reference related equipment:
```markdown
**Uses:** [Velleman K8200 3D Printer](../../Test%20Equipment/velleman-k8200/)
**Requires:** Components from [Programmable Devices/](../../Programmable%20Devices/)
```

## Project Naming Conventions

- Use lowercase with hyphens: `diy-trekpak-dividers`, `fpga-cpu-design`, `xbee-mesh-network`
- Be descriptive but concise
- Avoid generic names like `project1` or `test`

## When to Create a Project Directory

Create a project directory when:
1. **Multi-session work** - Project spans multiple work sessions
2. **Significant documentation** - Needs schematics, code, photos, build log
3. **Material tracking** - Complex BOM or parts from multiple sources
4. **Reference value** - Worth documenting for future similar projects
5. **Active implementation** - Moving from "idea" to "building"

## When to Keep in projects/project-ideas.md

Keep in projects/project-ideas.md when:
1. **Early planning** - Still deciding on approach
2. **Backlog** - Not currently working on it
3. **Simple concept** - Can be described in a few bullets
4. **Future consideration** - "Someday maybe" status

## Photo Organization

If including photos:
- Use descriptive filenames: `YYYY-MM-DD-description.jpg`
- Example: `2026-01-15-divider-prototype.jpg`
- Keep original resolution
- Store in `photos/` subdirectory

## Code Organization

If including firmware/software:
- Store in `code/` subdirectory
- Include README with build instructions
- Document dependencies and toolchain
- Include comments and version info

## Status Tracking

Use consistent status labels:
- **Planning** - Designing, researching, not yet started building
- **Materials Acquired** - Parts obtained, ready to begin
- **In Progress** - Actively building
- **Testing** - Built, now testing/debugging
- **Completed** - Finished and working
- **On Hold** - Paused, may resume later
- **Abandoned** - Discontinued, document why

## TODO.md Integration

Whenever creating or updating a project, add discovered work to TODO.md:

```markdown
### [Project Name] - [Phase if applicable]
- [ ] [Specific task discovered]
- [ ] [Next task needed]
```

Examples:
```markdown
### FPGA CPU Design - Phase 2
- [ ] Optimize timing constraints
- [ ] Test on hardware

### Home Automation - Integration
- [ ] Add Z-Wave node healing automation
- [ ] Create backup/restore procedure
```

This ensures:
- Discovered work is tracked and not lost
- Tasks can be prioritized across all projects
- Progress is visible in TODO.md history
- Related work can reference the task source

## Integration with workshop-capabilities.md

When a project demonstrates new capabilities:
```markdown
# In workshop-capabilities.md

## Recent Projects
- [DIY TrekPak Dividers](projects/diy-trekpak-dividers/) - Custom case organization system
- [XBee Mesh Network](projects/xbee-mesh-network/) - Wireless sensor network
```

## Protocol Maintenance & Version Control

### Keeping projects/README.md Current

The [projects/README.md](../projects/README.md) file serves as the **master index** of all projects and their documentation protocols. When creating or updating a project:

1. **Add/update project entry** in the Quick Reference table
2. **Create/update detailed section** under Projects by Category
3. **Verify protocol version** being used (check protocol footer for latest version number)
4. **Document git hash** of the protocol version for change tracking

### Detecting Protocol Changes with Git

Use git hashes to track when protocols are updated:

```bash
# Get the git hash of a specific protocol version
git log -1 --format="%H %ad %s" .claude/protocols/PROJECTS_PROTOCOL.md

# Document in projects/README.md as:
# | [PROJECTS_PROTOCOL.md](path) | Description | 1.1 | abc1234 |

# Later, detect if protocol has changed:
git log -1 --format="%H" .claude/protocols/PROJECTS_PROTOCOL.md
# Compare returned hash to documented hash above
```

### Template for Documenting Protocol Hash

When tracking protocol versions in projects/README.md, include:

```markdown
| [PROJECTS_PROTOCOL.md](../.claude/protocols/PROJECTS_PROTOCOL.md) | Creating & documenting projects | 1.1 | abc1234 |
#                                                                                                            ^ git hash
```

### When to Update projects/README.md

**Update immediately when:**
- Creating a new project
- Moving project from ideas → active
- Changing project status (Planning → In Progress, etc.)
- Updating protocol version (add hash in footer)
- Adding/removing projects

**Update quarterly when:**
- Running PROJECT_REVIEW_PROTOCOL
- Reviewing protocol applicability
- Checking for protocol updates from other team members

### Verifying Protocol Currency

Before following a protocol, verify it's the latest version:

```bash
# Check footer of protocol file for version and date
grep -E "^\\*Protocol Version:|^\\*Last Updated:" PROTOCOL_FILE.md

# If unsure, check git history:
git log -1 --format="%ai %s" .claude/protocols/PROTOCOL_FILE.md
```

### Template Maintenance

If this protocol is updated:
1. Increment version number (1.0 → 1.1)
2. Update "Last Updated" date
3. Document changes in Changelog section (if present)
4. Update projects/README.md with new version number and git hash
5. Create descriptive commit: "Update PROJECTS_PROTOCOL.md to version 1.1"

## Examples

See existing projects:
- [projects/diy-trekpak-dividers/](../projects/diy-trekpak-dividers) - Workshop organization project
- [projects/fpga-cpu-design/](../projects/fpga-cpu-design) - FPGA development project
- [projects/home-automation/](../projects/home-automation) - Smart home integration

## Related Protocols

For comprehensive research projects, see:
- [TECHNICAL-RESEARCH-PROTOCOL.md](TECHNICAL-RESEARCH-PROTOCOL.md) - For multi-phase research documentation

For overall project portfolio review:
- [PROJECT_REVIEW_PROTOCOL.md](PROJECT_REVIEW_PROTOCOL.md) - Quarterly inventory and status reviews

---

*Protocol Version: 1.2*
*Last Updated: 2026-01-16*

### Changelog

**v1.2 (2026-01-16)**
- Added "TODO.md Integration" section
- Made it standard to track discovered work when creating/updating projects
- Provided examples for adding project work to TODO.md
- Explained how this contributes to overall project progress tracking

**v1.1 (2026-01-16)**
- Added section: "Protocol Maintenance & Version Control"
- Added guidance for keeping projects/README.md current
- Added git hash tracking for detecting protocol updates
- Added templates for documenting protocol versions
- Added schedule for updating projects/README.md
- Added verification procedures for checking protocol currency
- Added "Related Protocols" section

**v1.0 (January 2026)**
- Initial protocol specification
- Directory structure guidance
- README.md template
- Build log format
- Parts list format
- Cross-reference guidelines
- Project naming conventions
- Status tracking guide
