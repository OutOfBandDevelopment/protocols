# TODO.md Archival Protocol

**Purpose:** Keep TODO.md manageable by archiving completed work to `./changes/` directory, maintaining performance and reducing token usage.

## Core Principle

TODO.md is a **working task list**, not a permanent record. Completed tasks should be archived to preserve history while keeping the active task list short and focused.

## File Structure

```
./
├── TODO.md                          # Active tasks only
└── changes/
    ├── README.md                    # Index of all change logs
    ├── fpga-cpu-initial-setup.md    # Specific project milestone
    ├── test-equipment-inventory-2026-01.md  # Time-based archive
    └── analog-computers-research.md # Topic-based archive
```

## TODO.md Guidelines

### Maximum Length

- **Target:** 50-100 lines, 10-20 active tasks
- **Warning threshold:** 150 lines, 30+ tasks
- **Critical threshold:** 200+ lines - immediate archival required

### Content to Keep

✅ **Active tasks** - Currently in progress or next up
✅ **Blocked tasks** - With clear blocker description
✅ **High-priority backlog** - Next 5-10 items
✅ **Context notes** - Brief notes for active work

### Content to Archive

❌ **Completed tasks** - More than 5-10 completed items
❌ **Cancelled tasks** - No longer relevant
❌ **Completed milestones** - Major topic finished
❌ **Old context notes** - For completed work

## When to Archive

### Immediate Triggers

Archive to `./changes/` when:

1. **Completed task accumulation** - More than 10 completed tasks in TODO.md
2. **Topic completion** - A major topic/project phase is done
3. **Monthly milestone** - End of month with significant completed work
4. **Performance issues** - TODO.md exceeds 150 lines
5. **Context switch** - Moving to completely different work area

### Archival Frequency

- **Minimum:** Every 20-30 completed tasks
- **Recommended:** Each major topic/milestone completion
- **Maximum wait:** Monthly, even if incomplete

## Archive File Naming

### Format

```
changes/{major-topic}-{minor-topic}[-{optional-date}].md
```

### Naming Patterns

**Project-based:**
- `fpga-cpu-initial-setup.md` - Specific milestone
- `sap1-computer-memory-controller.md` - Component work
- `home-automation-platform-migration.md` - Major change

**Inventory-based:**
- `test-equipment-inventory-2026-01.md` - Dated snapshot
- `programmable-devices-fpga-boards.md` - Category focus
- `expansion-boards-documentation.md` - Specific collection

**Topic-based:**
- `analog-computers-research.md` - Research phase
- `project-documentation-restructure.md` - Meta work
- `workshop-organization.md` - Ongoing improvements

**Time-based (use when topic spans multiple months):**
- `weekly-maintenance-2026-w02.md`
- `repository-updates-2026-01.md`

### Naming Guidelines

- Use lowercase with hyphens
- Major topic = high-level category (project name, inventory type, research area)
- Minor topic = specific component or phase
- Add date suffix for:
  - Time-based archives (monthly inventory updates)
  - Second archive of same topic (avoid topic-2, use topic-2026-02)
  - Ongoing maintenance work

## Archive File Format

### Standard Template

```markdown
# {Major Topic}: {Minor Topic}

Brief description of what was accomplished (1-2 sentences).

## Summary

- **Date range:** YYYY-MM-DD to YYYY-MM-DD
- **Total tasks completed:** X
- **Key outcomes:** Brief list

## Completed Tasks

### {Subsection if needed}

- [x] Task description with outcome
- [x] Another task - specific result achieved
- [x] Task with complications - what was learned

### {Another subsection}

- [x] Task description
- [x] Task description

## Files Changed

- `path/to/file.md` - What changed
- `path/to/another/file.md` - What changed

## Equipment/Resources Used

- [Equipment Name](link to equipment doc) - How it was used
- Tools, references, etc.

## Challenges & Solutions

### Challenge: {Problem encountered}
**Solution:** How it was resolved
**Learning:** What was learned

## Next Steps

- [ ] Follow-up task moved back to TODO.md
- [ ] Future work identified
- [ ] Blocked on: {dependency}

## Cross-References

- Related project: [link]
- Related equipment: [link]
- Related documentation: [link]

---

*Archived: YYYY-MM-DD*
*Git hash: {hash if available}*
```

### Minimal Template (for simple archives)

```markdown
# {Major Topic}: {Minor Topic}

Brief description.

## Completed Tasks

- [x] Task 1
- [x] Task 2
- [x] Task 3

## Files Changed

- `file1.md` - Added X
- `file2.md` - Updated Y

---

*Archived: YYYY-MM-DD*
```

## Archival Process

### Step 1: Identify Archive Candidates

Review TODO.md for:
- Completed tasks (marked with ✓, ✅, or [x])
- Old tasks no longer relevant
- Completed topic clusters

### Step 2: Group by Topic

Organize tasks into logical groups:
- Single project/feature
- Equipment inventory work
- Research phase
- Infrastructure/meta work

### Step 3: Create Archive File

```bash
# Create changes directory if needed
mkdir -p changes

# Create archive file with appropriate name
touch changes/topic-name.md
```

### Step 4: Document Completed Work

Using the template above:
1. List all completed tasks
2. Document files changed
3. Note challenges and solutions
4. List equipment/resources used
5. Add cross-references

### Step 5: Update TODO.md

Remove archived content from TODO.md:
- Delete completed tasks
- Move "next steps" back to TODO.md active section
- Keep any blocked or future tasks

### Step 6: Update Changes Index

Update `changes/README.md` with new archive entry.

### Step 7: Commit

```bash
git add TODO.md changes/
git commit -m "Archive completed tasks: {topic-name}"
```

## Changes Directory Index

Maintain `changes/README.md` as master index:

```markdown
# Change Log Index

Historical record of completed work archived from TODO.md.

## Table of Contents

- [2026](#2026)
  - [January](#january-2026)
- [By Category](#by-category)
  - [FPGA Projects](#fpga-projects)
  - [Equipment Inventory](#equipment-inventory)

## 2026

### January 2026

| Date | Topic | Description |
|------|-------|-------------|
| 2026-01-07 | [Project Index Creation](project-index-creation.md) | Created comprehensive project inventory |
| 2026-01-05 | [Test Equipment Docs](test-equipment-inventory-2026-01.md) | Documented oscilloscopes and DMMs |

## By Category

### FPGA Projects

- [FPGA CPU Initial Setup](fpga-cpu-initial-setup.md) - 2026-01-03
- [Zynq SoC Exploration](zynq-soc-exploration.md) - 2026-01-04

### Equipment Inventory

- [Test Equipment - January 2026](test-equipment-inventory-2026-01.md)
- [Programmable Devices - FPGA Boards](programmable-devices-fpga-boards.md)

---

*Last updated: 2026-01-07*
```

## TODO.md Maintenance

### Weekly Review

1. Count completed tasks
2. If > 10 completed, plan archival
3. Remove cancelled/irrelevant tasks

### After Each Major Milestone

1. Archive completed topic immediately
2. Update TODO.md with next phase
3. Create cross-reference in archive file

### Monthly Cleanup

1. Archive all completed tasks
2. Review blocked tasks - still relevant?
3. Re-prioritize backlog
4. Update changes/README.md index

## Example: Before vs After

### TODO.md Before Archival (Too Long - 250 lines)

```markdown
# TODO

## In Progress

- [ ] Working on FPGA CPU design
- [x] Set up Vivado project
- [x] Create initial SystemVerilog modules
- [x] Test ALU component
- [x] Debug timing issues
- [x] Add register file
- [x] Implement program counter
...
(100+ more lines of completed tasks)

## Equipment Inventory

- [x] Document Arty A7-100
- [x] Download datasheets
- [x] Create pinout table
- [x] Test with Vivado
...
(50+ more completed tasks)

## Done

- [x] Old task from December
- [x] Another old task
...
(100+ more old completed tasks)
```

### TODO.md After Archival (Clean - 50 lines)

```markdown
# TODO

## Active Tasks

- [ ] Working on FPGA CPU design
  - Completed initial setup (see changes/fpga-cpu-initial-setup.md)
  - Next: Implement control unit

## Equipment Inventory

- [ ] Document new SCPI equipment
- [ ] Update test equipment index

## Backlog

- [ ] Zynq SoC project
- [ ] Analog computer experiments

---

## Recent Archives

- [FPGA CPU Initial Setup](changes/fpga-cpu-initial-setup.md) - 2026-01-07
- [Equipment Inventory - January](changes/test-equipment-inventory-2026-01.md) - 2026-01-05

*Last cleaned: 2026-01-07*
```

### Archive File Created

**File:** `changes/fpga-cpu-initial-setup.md`

```markdown
# FPGA CPU Design: Initial Setup

Project initialization and development environment setup for custom CPU on Arty A7-100.

## Summary

- **Date range:** 2026-01-03 to 2026-01-07
- **Total tasks completed:** 15
- **Key outcomes:** Vivado project configured, initial SystemVerilog modules working, ALU tested

## Completed Tasks

### Development Environment

- [x] Set up Vivado project for Arty A7-100
- [x] Configure constraints file for board
- [x] Test synthesis and implementation flow

### RTL Design

- [x] Create initial SystemVerilog modules (ALU, registers, PC)
- [x] Test ALU component with testbench
- [x] Debug timing issues in combinational logic
- [x] Add register file with read/write ports
- [x] Implement program counter with increment logic
- [x] Create top-level module skeleton

...

---

*Archived: 2026-01-07*
```

## Performance Metrics

Track TODO.md length:

```bash
# Line count
wc -l TODO.md

# Task count
grep -c "^- \[" TODO.md

# Completed task count
grep -c "^- \[x\]" TODO.md
```

**Targets:**
- TODO.md: < 100 lines
- Active tasks: 10-20
- Completed tasks: < 10 (archive when exceeded)

## Integration with Git

### Commit Message Format

```
Archive completed tasks: {topic-name}

- Archived {N} completed tasks to changes/{filename}.md
- Updated TODO.md to focus on active work
- Cross-referenced in changes/README.md
```

### Git Hash in Archives

Include git hash in archive files:

```bash
# Get current commit hash
git rev-parse --short HEAD

# Add to archive file
echo "*Git hash: $(git rev-parse --short HEAD)*" >> changes/topic.md
```

## Emergency Cleanup

If TODO.md exceeds 300 lines:

1. **Immediately** archive all completed tasks
2. Create date-based archive: `changes/emergency-cleanup-YYYY-MM-DD.md`
3. Move everything completed to archive
4. Keep only:
   - Current in-progress task
   - Next 3-5 tasks in backlog
5. Add note in TODO.md: "See changes/emergency-cleanup-YYYY-MM-DD.md for archived work"

---

*Protocol version: 1.0*
*Created: 2026-01-07*
