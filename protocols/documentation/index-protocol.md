# Index Protocol

Standard protocol for creating and maintaining directory/section indexes across the repository.

## Executive Summary

This protocol ensures that all directory and section indexes follow a consistent pattern for easy navigation and discovery. Each index serves as the entry point to understanding what's in a directory and how to use its contents.

**Key principle:** Every major directory should have a focused, well-organized index (typically `README.md` in that directory) following this standard format.

## Table of Contents

- [When to Create an Index](#when-to-create-an-index)
- [Index Structure](#index-structure)
- [Index Categories](#index-categories)
- [Directory-Specific Guidelines](#directory-specific-guidelines)
- [Maintaining Indexes](#maintaining-indexes)
- [Version Tracking](#version-tracking)
- [Examples](#examples)

---

## When to Create an Index

Create a new index when:
- **Major directory created** - Any top-level or significant subdirectory
- **Content exceeds 5 items** - If a directory/section has multiple items, index them
- **Users need navigation** - When people need to find things or understand structure
- **Growing collection** - When it starts as 1-2 items but grows over time

**Don't create indexes for:**
- Temporary directories
- Single-file directories
- Auto-generated content directories

## Index Structure

### Header & Executive Summary

```markdown
# [Index Title]

[1-2 sentence description of what's in this index and its purpose]

**Quick links:** [Optional: 3-5 most important links/sections]
```

### Table of Contents

```markdown
## Table of Contents

- [Section 1](#section-1)
- [Section 2](#section-2)
- [Section 3](#section-3)
```

**Requirements:**
- Must be present in every index
- Must include all major sections
- Must use markdown anchor links (`#section-name`)
- Should be concise (5-15 entries typically)

### Main Content Sections

Organize content by topic. Each section should include:

```markdown
## Section Name

Brief description (1-2 sentences) of what this section covers.

### Subsection (if needed)

[Content]

| Item | Description | Status | Git Hash |
|------|-------------|--------|----------|
| ... | ... | ... | abc1234 |
```

**Git Hash Column (REQUIRED):**
- Include git hash (first 7 characters) of the most recent commit affecting that item
- Use: `git log -1 --format="%h" -- path/to/item`
- Enables easy change detection: `git diff old-hash new-hash`
- Makes it obvious when items have been updated
- Alternative: Include hash in item name/description if table not used

### Footer

```markdown
---

*Index Version: 1.0*
*Last Updated: 2026-01-16*
*Scope: [What this index covers]*
```

---

## Index Categories

### Type A: Simple Index (5-20 items)

**Used for:**
- Equipment collections
- Project lists
- Documentation files

**Structure:**
- Executive summary (1-2 sentences)
- Table of Contents
- Quick Reference table
- Optional: Categories section for organization

**Example:** `projects/README.md`

### Type B: Hierarchical Index (20+ items organized by category)

**Used for:**
- Large collections
- Equipment inventories
- Protocol collections

**Structure:**
- Executive summary
- Table of Contents
- Quick Reference (category summary)
- Detailed sections by category
- Cross-references
- Maintenance notes

**Example:** `.claude/protocols/README.md`

### Type C: Navigation Index (linking multiple subsections)

**Used for:**
- Directory hierarchies
- Research collections
- Multi-topic documentation

**Structure:**
- Executive summary
- Navigation guide / decision tree
- Main directories/sections
- How to navigate
- Quick-start paths

**Example:** Research project index

---

## Directory-Specific Guidelines

### `.claude/protocols/`

**Purpose:** Index of all 17 documentation protocols

**Structure:**
- Executive summary (what protocols are)
- Quick reference (decision tree for finding right protocol)
- Categorized table (organized by type: equipment, projects, maintenance)
- Version tracking table (showing all protocol versions)
- Maintenance section (how to keep this index current)

**Maintained by:** Protocol developers
**Updated when:** New protocol created or protocol version changes

### `projects/`

**Purpose:** Index of all active, completed, and planned projects

**Structure:**
- Executive summary (what projects exist)
- Quick reference table (all projects with category/status)
- Detailed section per project (organized by category)
- Status summary (count of projects in each status)
- Cross-references (to equipment, research, etc.)

**Maintained by:** Project owners
**Updated when:** New project created or project status changes

### Top-Level Directories (Programmable Devices, Test Equipment, etc.)

**Purpose:** Index of equipment/items in that category

**Structure:**
- Executive summary (what category contains)
- Quick reference table (all items with key specs)
- Optional: Detailed sections if needed
- Organization note (how subdirectories are arranged)

**Maintained by:** Equipment curators
**Updated when:** New equipment added or specs change

### Research Collections

**Purpose:** Navigation to research materials

**Structure:**
- Executive summary (research focus)
- Navigation guide (how to find things)
- Main sections (by topic or source)
- How to use this collection
- Contribution guidelines

**Maintained by:** Research owners
**Updated when:** New research added

---

## Maintaining Indexes

### TODO.md Updates

**Whenever discovering work during index creation or maintenance:**

1. Add new items to TODO.md under appropriate section
2. Include context: what directory/index, what needs to be done
3. Include git hash of the indexed item/directory (for later `git diff`)
4. Update "Last updated" timestamp
5. Add "Session Discoveries" section if new discovery type

Examples:
```markdown
### Directory Indexes (INDEX_PROTOCOL.md)
- [ ] Create README.md for Programmable Devices/ (abc1234)
- [ ] Add cross-references to device projects

### [Project/Research] Phase 2
- [ ] Subtask 1 (def5678)
- [ ] Subtask 2 (ghi9012)
```

**Why include git hashes:**
- Later review can use: `git diff abc1234 HEAD -- path/to/item`
- See what changed without reviewing entire index
- Track how indexed items evolve over time
- Makes it obvious which items have been modified

This ensures discovered work doesn't get lost and contributes to overall progress tracking.

### Update Schedule

**Weekly:** When adding items to the collection

**Monthly:** Review for accuracy, broken links

**Quarterly:** Comprehensive review, restructuring if needed

### Update Checklist

When updating an index:

- [ ] Add/remove items from quick reference table
- [ ] Update table of contents if structure changed
- [ ] Verify all links are working
- [ ] Update "Last Updated" date
- [ ] Update index version if significant changes made
- [ ] Add changelog entry (if version incremented)
- [ ] Verify total counts are accurate (projects, protocols, etc.)

### Version Numbering

```
1.0 = Initial index creation
1.1 = Minor updates (new items, typos, links)
1.2 = Moderate updates (reorganization, new sections)
2.0 = Major restructuring (new pattern, significant changes)
```

**Only increment version when:**
- Adding new sections
- Reorganizing structure
- Changing how items are categorized
- Major content additions

**Do NOT increment version for:**
- Adding/removing items from existing sections
- Fixing typos
- Updating dates
- Clarifying descriptions

---

## Version Tracking

Each index footer should include:

```markdown
---

*Index Version: 1.0*
*Last Updated: 2026-01-16*
*Total items: [Count of items indexed]*
*Scope: [What this index covers]*
```

### Master Index Table

The `.claude/protocols/README.md` file maintains a version tracking table for all major indexes:

| Index | Location | Version | Last Updated | Items |
|-------|----------|---------|--------------|-------|
| Projects | projects/README.md | 1.0 | 2026-01-16 | 21 |
| Protocols | .claude/protocols/README.md | 1.4 | 2026-01-16 | 18 |
| Programmable Devices | Programmable Devices/readme.md | 1.0 | TBD | TBD |

Update this table quarterly.

---

## Examples

### Minimal Index (5 items)

```markdown
# [Category] Index

Brief description of what's in this category.

## Table of Contents

- [Item 1](#item-1)
- [Item 2](#item-2)
- [Item 3](#item-3)

| Item | Description | Status |
|------|-------------|--------|
| Item 1 | Purpose | Active |
| Item 2 | Purpose | Reference |
| Item 3 | Purpose | Planning |

---

*Index Version: 1.0*
*Last Updated: 2026-01-16*
*Total items: 3*
```

### Standard Index (15 items, 3 categories)

```markdown
# [Category] Index

Description of what's in this category and purpose.

## Quick Reference

| Subcategory | Items | Status |
|------------|-------|--------|
| Sub A | 5 items | Active |
| Sub B | 5 items | Reference |
| Sub C | 5 items | Planning |

## Table of Contents

- [Subcategory A](#subcategory-a)
- [Subcategory B](#subcategory-b)
- [Subcategory C](#subcategory-c)
- [How to Add Items](#how-to-add-items)

## Subcategory A

[Items in category A]

## Subcategory B

[Items in category B]

## Subcategory C

[Items in category C]

## How to Add Items

Follow [CATEGORY_PROTOCOL.md](../.claude/protocols/CATEGORY_PROTOCOL.md)

---

*Index Version: 1.0*
*Last Updated: 2026-01-16*
*Total items: 15*
```

---

## Common Pitfalls

**Don't:**
- Mix multiple unrelated things in one index
- Create indexes for temporary directories
- Forget to update dates/versions
- Include outdated information
- Use inconsistent formatting

**Do:**
- Keep indexes focused and organized
- Update regularly (at least monthly)
- Include cross-references
- Follow the standard structure
- Explain how to use the index

---

## Related Protocols

- [PROJECTS_PROTOCOL.md](PROJECTS_PROTOCOL.md) - Creating project documentation
- [PROJECT_REVIEW_PROTOCOL.md](PROJECT_REVIEW_PROTOCOL.md) - Maintaining project indexes
- [TECHNICAL-RESEARCH-PROTOCOL.md](TECHNICAL-RESEARCH-PROTOCOL.md) - Research project documentation

---

*Protocol Version: 1.2*
*Last Updated: 2026-01-16*

## Changelog

**v1.2 (2026-01-16)**
- Made git hash tracking REQUIRED in all index tables
- Added git hash column template to Main Content Sections
- Documented git log command for retrieving commit hashes
- Explained git diff workflow for change detection
- Updated TODO.md examples to include git hashes
- Clarified why git hashes reduce review burden

**v1.1 (2026-01-16)**
- Added TODO.md Integration section
- Made it standard to track discovered work when creating/maintaining indexes
- Provided examples for adding index work to TODO.md
- Explained how this contributes to overall progress tracking

**v1.0 (2026-01-16)**
- Initial protocol specification
- Defined index structure with executive summary, TOC, content, footer
- Specified 3 types of indexes: Simple, Hierarchical, Navigation
- Added directory-specific guidelines
- Added version numbering standards
- Provided examples and common pitfalls
- Integrated with PROJECT_REVIEW_PROTOCOL for quarterly maintenance
