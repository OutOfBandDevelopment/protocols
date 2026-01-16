# Changelog Protocol

**Purpose:** Document repository changes in `./changes/` directory with session-based or topic-based change logs.

## Core Principle

Repository changes should be documented in focused, topic-based files in the `./changes/` directory rather than a single monolithic CHANGELOG.md file. This keeps change history organized, searchable, and manageable.

## File Structure

```
./
├── changes/
│   ├── README.md                                    # Master index of all changes
│   ├── repository-reorganization-2026-01-07.md     # Session-based changelog
│   ├── diagrams-patterns-rename-2026-01-07.md      # Topic-based changelog
│   ├── ai-ml-notes-reorganization-2026-01-07.md    # Topic-based changelog
│   └── projects-index-creation-2026-01-07.md       # Topic-based changelog
```

## When to Create a Changelog

### Session-Based Changelogs

Create a session-based changelog when:
- Completing a major multi-topic work session
- Session includes 10+ significant file changes
- Multiple unrelated topics were addressed
- User explicitly requests "create a changelog"

**Naming:** `repository-update-YYYY-MM-DD.md` or `session-summary-YYYY-MM-DD.md`

### Topic-Based Changelogs

Create topic-based changelogs when:
- Completing a focused reorganization (e.g., moving directories)
- Major documentation updates for a specific area
- Equipment inventory additions (batch of 5+ devices)
- Project milestone completions

**Naming:** `{topic-name}-YYYY-MM-DD.md`

Examples:
- `diagrams-patterns-rename-2026-01-07.md`
- `notes-directory-cleanup-2026-01-07.md`
- `programmable-devices-inventory-2026-01.md`
- `sap1-computer-completion-2026-01-15.md`

## Changelog File Format

### Standard Template

```markdown
# {Topic Title}

Brief 1-2 sentence description of what changed.

**Session Date:** YYYY-MM-DD

## Summary

- **Scope:** What area of repository was affected
- **Files added:** X new files
- **Files modified:** Y files updated
- **Files moved:** Z relocations
- **Files deleted:** N removals

## Changes

### Added

- **path/to/new-file.md** - Description of what was added and why
- **another/new/directory/** - Purpose of new directory

### Modified

- **path/to/modified-file.md** - What changed and why
  - Updated section X
  - Added cross-references to Y

### Moved

- **old/path/file.md** → **new/path/file.md** - Reason for move

### Deleted

- **old/file.md** - Why it was removed (merged, obsolete, etc.)

### Renamed

- **OldName.md** → **new-name.md** - Applied naming convention

## Files Changed

Detailed list of all files affected:

```
changes/
├── new-file-1.md (added)
├── modified-file.md (modified)
└── moved-file.md (moved from old/location)
```

## Impact

- **Projects affected:** List of projects impacted
- **Equipment documentation:** What equipment docs were updated
- **Cross-references:** What links needed updating
- **Breaking changes:** Any changes that affect existing workflows

## Related Work

- Related to archive: [archive-name.md](archive-name.md)
- Addresses TODO items: List of completed TODO items
- Follows protocol: [PROTOCOL_NAME.md](../protocols/PROTOCOL_NAME.md)

---

*Change log created: YYYY-MM-DD*
```

### Minimal Template (for simple changes)

```markdown
# {Topic Title}

Brief description.

**Session Date:** YYYY-MM-DD

## Changes

### Added
- `file1.md` - Description
- `file2.md` - Description

### Modified
- `file3.md` - What changed

### Moved
- `old/path` → `new/path` - Why

---

*Change log created: YYYY-MM-DD*
```

## Changes Directory Index

Maintain `changes/README.md` as master index with two organizations:

### By Date (Primary)

```markdown
# Change Log Index

Historical record of repository changes organized by date and topic.

## 2026

### January 2026

| Date | Topic | Type | Description |
|------|-------|------|-------------|
| 2026-01-07 | [Repository Reorganization](repository-reorganization-2026-01-07.md) | Session | Major Notes/ cleanup and project documentation |
| 2026-01-07 | [DiagramsAndPatterns Rename](diagrams-patterns-rename-2026-01-07.md) | Topic | Applied lowercase-hyphen naming convention |
| 2026-01-05 | [Projects Index Creation](projects-index-creation-2026-01-07.md) | Topic | Created comprehensive 18-project index |
```

### By Category (Secondary)

```markdown
## By Category

### Repository Organization
- [Repository Reorganization - 2026-01-07](repository-reorganization-2026-01-07.md)
- [Notes Directory Cleanup - 2026-01-07](notes-cleanup-2026-01-07.md)

### Project Documentation
- [Projects Index Creation - 2026-01-07](projects-index-creation-2026-01-07.md)
- [AI/ML Research Project - 2026-01-07](ai-ml-research-project-2026-01-07.md)

### Equipment Inventory
- [Programmable Devices - January 2026](programmable-devices-inventory-2026-01.md)
- [Device Lab Additions - 2026-01-07](device-lab-additions-2026-01-07.md)
```

## When to Update vs Create

### Update Existing Changelog

Update an existing changelog (within same day) when:
- Follow-up work on same topic
- Minor corrections or additions
- Completing remaining tasks from same session

### Create New Changelog

Create a new changelog when:
- Different day
- Different topic/focus area
- Major scope change from previous work

## Integration with TODO.md Archives

Changelogs and TODO archives serve different purposes:

**TODO Archives (`changes/{topic}.md`):**
- Focus on TASKS completed
- Track challenges and solutions
- Document equipment/resources used
- Next steps and follow-ups

**Changelogs (`changes/{topic}-YYYY-MM-DD.md`):**
- Focus on CHANGES to repository
- Track files added/modified/moved/deleted
- Document structural changes
- Impact on cross-references

**Both can coexist** - A major project milestone might generate both:
- Archive: `changes/fpga-cpu-initial-setup.md` (task tracking)
- Changelog: `changes/fpga-cpu-docs-2026-01-15.md` (documentation changes)

## Examples

### Example 1: Session-Based Changelog

**File:** `changes/repository-reorganization-2026-01-07.md`

```markdown
# Repository Reorganization - Session 1

Major reorganization of Notes/ directory, project documentation, and equipment inventory updates.

**Session Date:** 2026-01-07

## Summary

- **Scope:** Repository-wide reorganization
- **Files added:** 12 new project READMEs, 4 device documentation files
- **Files modified:** 8 index files updated with cross-references
- **Files moved:** 25+ files from Notes/ to appropriate locations
- **Files deleted:** 1 empty directory removed

## Changes

### Added

**Project Documentation:**
- **projects/README.md** - Comprehensive 18-project index
- **projects/sbc-cluster/README.md** - ARM cluster documentation
- **projects/radex-one-protocol-reverse-engineering/README.md** - Geiger counter protocol
- **projects/favero-fencing-scoring-system/README.md** - Scoring machine interface
- **projects/vector-search-sqlserver/README.md** - SQL Server LSH implementation
- **projects/gadget-kit-ideas.md** - Computer kits and test equipment wishlist
- **projects/streaming-channel-planning.md** - YouTube/Twitch channel planning

**Device Documentation:**
- **Programmable Devices/netv-fpga/README.md** - NeTV FPGA video overlay device
- **Device Lab/laptops/microsoft-surface-book-2-15/README.md** - Surface Book specs
- **Device Lab/vr-ar/microsoft-hololens/README.md** - HoloLens v1 documentation
- **Device Lab/sensors/xbox360-kinect/README.md** - Kinect v1 with PC adapter
- **Device Lab/sensors/xboxone-kinect/README.md** - Kinect v2 documentation

### Moved

**From Notes/ to Projects:**
- `Notes/hardware/74-181/` → `projects/sap-1-computer/reference-74181-alu/`
- `Notes/hardware/3D Printer/` → `Test Equipment/velleman-k8200/notes/`
- `Notes/hardware/Field Programmable Analog Array/` → `projects/analog-computer-experiments/fpaa-research/`
- `Notes/software/Kubernetes/` → `projects/devops-playground-cluster/kubernetes-notes/`
- `Notes/Favero/` → `projects/favero-fencing-scoring-system/reference/`
- `Notes/gadgets and gizmos/RadexOneReverseEngineerNotes.md` → `projects/radex-one-protocol-reverse-engineering/protocol-notes.md`
- `Notes/Gadgets.md` → `projects/gadget-kit-ideas.md`
- `Notes/Stream.md` → `projects/streaming-channel-planning.md`

**From Notes/ to Top Level:**
- `Notes/software/DiagramsAndPatterns/` → `/diagrams-and-patterns/`

**From Root to Projects:**
- `/HammingDistances/` → `projects/vector-search-sqlserver/HammingDistances/`
- `/Semantic Search/` → `projects/vector-search-sqlserver/Semantic Search/`

### Modified

**Index Files:**
- **Notes/README.md** - Added "Moved to Projects" and "Moved to Top Level" sections
- **projects/README.md** - Created with 18 projects, quick reference table, cross-references
- **Device Lab/readme.md** - Added VR/AR & Motion Sensing category
- **Programmable Devices/readme.md** - Added NeTV FPGA board
- **CLAUDE.md** - Updated to reflect diagrams-and-patterns top-level location

**Project Status Updates:**
- ATtiny2313 LED Clock → Completed
- MM-8000K Trainer UI → On Hold
- DIY TrekPak Dividers → Reference/As Needed
- Favero Fencing System → Completed

### Deleted

- `Notes/software/AI ML Stuff/` (empty directory after moving files)

## Impact

**Projects affected:** All 18 projects now have comprehensive documentation
**Equipment documentation:** 4 new devices added to Device Lab, 1 to Programmable Devices
**Cross-references:** 20+ files updated with correct paths after reorganizations
**Breaking changes:** None - all moves preserved content with updated cross-references

## Related Work

- Created TODO.md Infrastructure (Priority) section
- Clarified PR4100 vs DevOps Playground Cluster architecture
- Cleaned Notes/ directory to focus on general reference materials

---

*Change log created: 2026-01-07*
```

### Example 2: Topic-Based Changelog

**File:** `changes/diagrams-patterns-rename-2026-01-07.md`

```markdown
# DiagramsAndPatterns Naming Convention Update

Renamed all files and directories in diagrams-and-patterns/ to match repository lowercase-with-hyphens convention.

**Session Date:** 2026-01-07

## Summary

- **Scope:** diagrams-and-patterns/ directory structure
- **Files renamed:** 3 markdown files
- **Directories renamed:** 5 subdirectories
- **Files modified:** 1 (README.md for cross-references)

## Changes

### Renamed

**Files:**
- `FutureIdeas.md` → `future-ideas.md`
- `SuggestedGuidelines.md` → `suggested-guidelines.md`
- `TableOfContents.md` → `table-of-contents.md`

**Directories:**
- `ArchitecturePatterns/` → `architecture-patterns/`
- `DesignPatterns/` → `design-patterns/`
- `Examples/` → `examples/`
- `Templates/` → `templates/`
- `Designs/` → `designs/`

### Modified

- **diagrams-and-patterns/README.md** - Updated file references to use lowercase filenames

## Impact

**Breaking changes:** Internal links within diagrams-and-patterns/ may need updates (future work)
**Cross-references:** All external references to diagrams-and-patterns files updated
**Consistency:** Now matches repository-wide naming convention

---

*Change log created: 2026-01-07*
```

## Changelog vs CHANGELOG.md

**DO NOT create a root-level CHANGELOG.md** - This becomes unwieldy and hard to maintain.

**DO create individual change logs in `./changes/`** with focused topics and clear naming.

**Exception:** A root-level CHANGELOG.md is acceptable ONLY if it's a simple index that points to files in `./changes/`:

```markdown
# Changelog

See [changes/](changes/) directory for detailed change logs.

## Recent Changes

- [2026-01-07: Repository Reorganization](changes/repository-reorganization-2026-01-07.md)
- [2026-01-07: DiagramsAndPatterns Rename](changes/diagrams-patterns-rename-2026-01-07.md)

For complete index, see [changes/README.md](changes/README.md).
```

## Process

### When User Requests "Create a Changelog"

1. **Assess scope:**
   - Single topic? → Topic-based changelog
   - Multiple topics? → Session-based changelog

2. **Create `changes/` directory if needed:**
   ```bash
   mkdir -p changes
   ```

3. **Write changelog file:**
   - Use appropriate template
   - Focus on WHAT changed, not WHY (tasks)
   - List all files affected
   - Document impact on cross-references

4. **Update or create `changes/README.md`:**
   - Add new entry to date table
   - Add to category section
   - Keep sorted by date (newest first)

5. **Optional: Create root CHANGELOG.md as index:**
   - Only if it doesn't exist
   - Keep it minimal - just points to changes/
   - Don't duplicate content

## Naming Convention

### Session-Based
```
{scope}-{date}.md

Examples:
- repository-reorganization-2026-01-07.md
- session-summary-2026-01-07.md
- weekly-maintenance-2026-w02.md
```

### Topic-Based
```
{topic-name}-{date}.md

Examples:
- diagrams-patterns-rename-2026-01-07.md
- ai-ml-notes-reorganization-2026-01-07.md
- projects-index-creation-2026-01-07.md
- netv-fpga-documentation-2026-01-07.md
```

### Guidelines
- Use lowercase with hyphens
- Include date suffix (YYYY-MM-DD)
- Topic name should be specific and descriptive
- Avoid generic names like "changes-2026-01-07.md"

## Changelog vs Archive Coexistence

Both TODO archives and changelogs can exist for the same work:

**Example: FPGA CPU Project**

```
changes/
├── fpga-cpu-initial-setup.md              # TODO archive (tasks)
└── fpga-cpu-documentation-2026-01-15.md   # Changelog (file changes)
```

**TODO Archive focuses on:**
- Tasks completed
- Challenges encountered
- Solutions found
- Next steps

**Changelog focuses on:**
- Files added/modified/moved/deleted
- Documentation structure changes
- Cross-reference updates
- Repository organization

## Performance Benefits

Individual change logs:
- ✅ Fast to search (grep through changes/)
- ✅ Easy to reference specific changes
- ✅ Smaller file sizes (better for context)
- ✅ Clear organization by topic and date
- ✅ Multiple people can work without conflicts

Single CHANGELOG.md:
- ❌ Becomes very long (>1000 lines)
- ❌ Merge conflicts if multiple editors
- ❌ Slow to search
- ❌ Hard to find specific changes
- ❌ High token usage when included in context

---

*Protocol Version: 1.0*
*Created: 2026-01-07*
