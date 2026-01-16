# CLAUDE.md Maintenance Protocol

**Purpose:** Keep CLAUDE.md concise and focused on critical context only, reducing token usage and improving performance.

## Core Principle

CLAUDE.md should contain **pointers and high-level guidance**, not detailed information. Detailed information belongs in:
- Index files (README.md in each directory)
- Topic-specific protocol files
- Equipment documentation
- Project files

## Maximum Length Guidelines

- **Target:** 300-500 lines (10,000-15,000 tokens)
- **Warning threshold:** 600 lines (18,000 tokens)
- **Critical threshold:** 800+ lines (24,000+ tokens) - immediate reduction required

## Content Structure

### Essential Sections (Keep)

1. **Repository Overview** (5-10 lines)
   - Brief description
   - Owner information with GitHub links

2. **Key Directories** (20-30 lines)
   - One-line description per major directory
   - Link to detailed README.md for each

3. **Documentation Protocols** (30-50 lines)
   - List protocols with when to use them
   - Link to full protocol files
   - No detailed instructions - those belong in protocol files

4. **Cross-References** (20-30 lines)
   - Links to main index files
   - Equipment inventory locations
   - Research collections

5. **File Naming Conventions** (10-15 lines)
   - Brief examples only

6. **Common Tasks** (10-15 lines)
   - Most frequent bash commands only
   - Link to detailed guides if needed

### Reduce or Remove

❌ **Detailed protocol instructions** - Move to `.claude/protocols/*.md`
❌ **Equipment lists** - Link to inventory files instead
❌ **Detailed examples** - Keep one example, move rest to protocol files
❌ **Step-by-step instructions** - Keep only in protocol files
❌ **Redundant information** - If it's in an index, just reference it
❌ **Historical notes** - Move to project READMEs or archive
❌ **Extended explanations** - Keep brief, link to details

## Refactoring Process

### When to Refactor

Refactor CLAUDE.md when:
- File exceeds 600 lines
- Adding major new section
- Quarterly maintenance review
- Performance degradation noticed

### Refactoring Steps

1. **Identify bloat**
   ```bash
   wc -l CLAUDE.md
   grep -n "^#" CLAUDE.md  # Find all section headers
   ```

2. **Extract detailed content**
   - Create or update topic-specific protocol files
   - Create or update index files
   - Move examples to protocol files

3. **Replace with references**
   - Replace detailed sections with: "See [protocol-name.md] for details"
   - Replace lists with: "See [inventory/README.md] for complete list"
   - Keep only the most critical example

4. **Update cross-references**
   - Ensure all referenced files exist
   - Verify links are correct

5. **Add archive note** (if removing historical content)
   ```markdown
   ## Archived Sections

   The following sections were moved to improve performance:
   - Detailed equipment specifications → See `Programmable Devices/README.md`
   - Protocol examples → See `.claude/protocols/*.md`
   - Project history → See `projects/README.md`

   *Archived: 2026-01-07*
   ```

## Example: Before vs After

### Before (Too Detailed - 150 lines)

```markdown
## Programmable Devices Protocol

When adding new development boards:

1. Create directory with device name
2. Download datasheets from manufacturer
3. Verify PDF is valid:
   ```bash
   file *.pdf
   ls -lh *.pdf
   ```
4. Create README.md with:
   - Overview
   - Specifications (detailed table)
   - Pinout (table and image)
   - Programming tools
   - Example projects
5. Update main README.md index
6. Cross-reference with related projects
...
(100+ more lines of detailed instructions)
```

### After (Concise - 15 lines)

```markdown
## Documentation Protocols

All protocols are located in `.claude/protocols/`.

### Programmable Devices
Follow `.claude/protocols/PROGRAMMABLE_DEVICES_DOCUMENTATION_PROTOCOL.md` when:
- Adding new development boards or microcontrollers
- Creating device README files with specifications
- Downloading and verifying datasheets/schematics

Key requirements:
- Each device gets its own directory with standardized README.md
- Download PDFs locally (datasheets, schematics, pinouts)
- Verify downloaded files are valid PDFs
```

## Index File Standards

All index files referenced from CLAUDE.md must include:

### Required Header

```markdown
# [Collection Name] Index

Brief description (1-2 sentences).

## Table of Contents

- [Category 1](#category-1)
- [Category 2](#category-2)
- [Quick Reference](#quick-reference)

## Quick Reference

| Item | Status | Key Info |
|------|--------|----------|
| ... | ... | ... |

[Main content sections...]

---

*Last updated: YYYY-MM-DD*
*Git hash: [hash if in git repo]*
```

### Required Elements

1. **Brief description** - One paragraph maximum
2. **Table of Contents** - For files > 100 lines
3. **Quick Reference Table** - For inventories
4. **Last updated date** - Always include
5. **Git hash** - Include if repository is under git

## Maintenance Schedule

### Weekly
- Check CLAUDE.md line count
- If over 600 lines, plan refactoring

### Monthly
- Review for outdated information
- Update cross-references
- Verify all referenced files exist

### Quarterly
- Full content audit
- Extract detailed content to protocol/index files
- Optimize for performance

## Performance Metrics

Track token usage impact:

```bash
# Rough token estimate (1 token ≈ 4 characters)
wc -c CLAUDE.md | awk '{print $1/4 " tokens (approx)"}'
```

**Target:** < 15,000 tokens for CLAUDE.md
**Combined target:** CLAUDE.md + all referenced protocol files < 50,000 tokens

## Migration Checklist

When moving content out of CLAUDE.md:

- [ ] Create/update target file (protocol or index)
- [ ] Verify target file has proper header (TOC, date, hash)
- [ ] Replace detailed content in CLAUDE.md with reference
- [ ] Test that reference link works
- [ ] Verify no information was lost
- [ ] Commit changes with descriptive message
- [ ] Update "Last updated" in CLAUDE.md

## Emergency Reduction

If CLAUDE.md exceeds 1000 lines and immediate reduction needed:

1. **Move all detailed protocols** to `.claude/protocols/`
2. **Replace equipment lists** with "See [Equipment]/README.md"
3. **Remove all examples** except most critical one per section
4. **Consolidate cross-references** into single section
5. **Archive historical notes** to project READMEs

Target: Reduce by 50% minimum in emergency mode.

---

*Protocol version: 1.0*
*Created: 2026-01-07*
