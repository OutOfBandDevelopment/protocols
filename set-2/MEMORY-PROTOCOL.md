# AI Assistant Memory Management Protocol

This document defines the protocol for managing AI assistant memory and context across the CadLink repository.

## Core Principle

**CLAUDE.md should be an ultra-minimal navigation index, NOT a content repository.**

**Philosophy**: Don't load it in memory if you don't need it right now. Look up information on-demand.

All information should be stored in specialized documentation files and loaded only when needed. CLAUDE.md is just a map showing where things are.

## File Organization Strategy

### Primary Memory File
- **`/CLAUDE.md`** - Ultra-minimal navigation index (target: <150 lines)
  - Project name and 2-3 sentence description
  - Links to documentation indexes ONLY
  - NO summaries, NO detailed content, NO recent work sections
  - Think: "table of contents" not "executive summary"

### Reference Documentation
- **`docs/reference/`** - Quick lookup reference docs (create as needed)
  - `overview.md` - Project overview, key characteristics, architecture summary
  - `status.md` - Current status of code reviews, user guides, projects
  - `quick-start.md` - Quick start guide, common commands, building/testing
  - `resources.md` - Links to all documentation categories with brief descriptions

### Current Production Documentation
- **`docs/current/`** - Current system references and operational guides
  - `quick-reference.md` - Fast lookup: key files, entry points, common commands
  - `repository-structure.md` - Directory layout and module organization
  - `building.md` - Build instructions and pipeline details
  - `testing.md` - Test execution and pipeline configuration
  - `dependencies.md` - Third-party libraries and versions
  - `development-tasks.md` - Common development workflows
  - `development-notes.md` - Code namespaces, services, processes
  - `internal-tools.md` - Internal utilities catalog
  - `troubleshooting.md` - Common issues and solutions

### Architecture Documentation
- **`docs/design/`** - Current architecture (C4 model, technical specifications)
  - `01-10-*.md` - Core architecture documents (already exist)
  - `best-practices.md` - Development best practices and patterns
  - `README.md` - Architecture documentation index

### Future Architecture Documentation
- **`docs/design/v-next/`** - Future architecture and modernization
  - `README.md` - v-next directory guide
  - `architectural-patterns.md` - Current pattern analysis
  - `v-next.md` - Modernization roadmap
  - `v-next-addendum.md` - Detailed patterns and examples (~7,400 lines)
  - `TODO.md` - v-next task tracking and decisions
  - `MEMORY.md` - v-next quick reference for AI assistants

### Code Review Documentation
- **`docs/code-review/`** - Comprehensive code review findings (63 phases, completed 2025-10-15)
  - `README.md` - Main index with top 10 critical issues and remediation roadmap
  - `PHASES-01-10-SUMMARY.md` - Initial review summary (Dependencies, Architecture, Security)
  - `PHASES-11-55-SUMMARY.md` - Deep-dive analysis summary (Security, HIPAA, Performance)
  - `PHASES-56-63-SUMMARY.md` - Gap analysis summary (DateTime, WCF, Database, Strings)
  - `REMEDIATION-TRACKER.md` - **START HERE for fixing issues** - Week-by-week tracking
  - `TODO.md` - Progress tracking for all 63 phases
  - `01-63-*.md` - Individual phase analysis documents (63 detailed reports)
  - `CODE-REVIEW-PROTOCOL.md` - Protocol for conducting code reviews
  - `BUILD-METRICS-PROTOCOL.md` - Build performance analysis protocol
  - `BUILD-VALIDATION-PROTOCOL.md` - **NEW** Build validation after code fixes

### User and Product Owner Documentation
- **`docs/user/`** (create as needed) - End-user documentation
- **`docs/product/`** (create as needed) - Product owner and stakeholder docs

## Context Loading Rules for AI Assistants

### Rule 1: Load Based on Task Type

**Working on Current Production Code**:
1. Read `/CLAUDE.md` (thin guide)
2. Read `/TODO.md` (current production tasks)
3. Read `docs/current/quick-reference.md` (fast lookup)
4. Load specific files from `docs/current/` as needed
5. **DO NOT** load `docs/design/v-next/` to conserve context

**Working on v-next Design**:
1. Read `/CLAUDE.md` (thin guide)
2. Read `docs/design/v-next/MEMORY.md` (v-next quick reference)
3. Read `docs/design/v-next/TODO.md` (v-next tasks)
4. Load specific sections of `docs/design/v-next/v-next-addendum.md` as needed

**Researching Architecture**:
1. Read `/CLAUDE.md` (thin guide)
2. Read `docs/design/README.md` (architecture index)
3. Load specific architecture documents (01-10) as needed

**Fixing Code Review Issues**:
1. Read `/CLAUDE.md` (thin guide)
2. Read `docs/code-review/README.md` (top 10 critical issues)
3. Read `docs/code-review/REMEDIATION-TRACKER.md` (task tracking)
4. Load specific phase documents (01-63) as needed for issue details
5. **DO NOT** load all 63 phase documents - use README for navigation

**Conducting New Code Review**:
1. Read `/CLAUDE.md` (thin guide)
2. Read `protocols/CODE-REVIEW-PROTOCOL.md` (methodology)
3. Read `docs/code-review/REVIEW-GAPS-ANALYSIS.md` (gap identification)
4. Review previous findings in phase summaries for patterns

### Rule 2: Just-In-Time Loading

Load detailed documentation only when specifically needed for the current task. Use CLAUDE.md and quick-reference files to locate information, then load the full document.

### Rule 3: Prioritize Summary Over Detail

When multiple files cover a topic, prefer:
1. Quick reference files first (summaries)
2. Main documentation files second (details)
3. Source code last (when implementation details needed)

## Memory Update Protocol

### When to Update Memory

Update documentation in the following situations:

1. **Architecture Changes**
   - Update `docs/design/` files when current architecture changes
   - Update `docs/design/v-next/` files when future design evolves
   - Update `docs/design/README.md` index if new documents added

2. **New Features or Components**
   - Update `docs/current/repository-structure.md` if new directories/modules added
   - Update `docs/current/dependencies.md` if new third-party libraries added
   - Update `docs/current/quick-reference.md` if new key files/entry points added

3. **Process Changes**
   - Update `docs/current/building.md` if build process changes
   - Update `docs/current/testing.md` if test process changes
   - Update `docs/current/development-tasks.md` if workflows change

4. **Session Completion**
   - Update `/CLAUDE.md` "Recent Session Work" (keep brief: 3-5 bullet points max)
   - Update `/TODO.md` or `docs/design/v-next/TODO.md` with task progress
   - Update `docs/design/v-next/MEMORY.md` if significant v-next work completed

5. **Code Review Work**
   - Update `docs/code-review/REMEDIATION-TRACKER.md` when issues are fixed
   - Update `docs/code-review/TODO.md` if new review phases added
   - Create new phase documents (XX-topic.md) for new findings
   - Update summary documents (PHASES-XX-YY-SUMMARY.md) if completing phase groups
   - Update `docs/code-review/README.md` if statistics or top issues change

### Update Checklist

Before ending a session, check:
- [ ] Is CLAUDE.md ultra-minimal? (target: <150 lines, HARD LIMIT: <200 lines)
- [ ] Does CLAUDE.md contain ONLY links, NO summaries or detailed content?
- [ ] Have summaries been moved to docs/reference/ instead?
- [ ] Are all detailed code examples in appropriate docs, not CLAUDE.md?
- [ ] Does quick-reference.md have the latest key files and entry points?
- [ ] Are task lists (TODO.md) up to date?
- [ ] Are new documents added to appropriate README.md indexes?

## Document Structure Standards

### Every Documentation File Should Have

1. **Header** - Title and purpose
2. **Table of Contents** - For files >100 lines
3. **Overview** - 2-3 sentence summary
4. **Sections** - Logically organized with clear headings
5. **Examples** - Code examples where appropriate
6. **Cross-References** - Links to related documents
7. **Last Updated** - Date stamp

### Quick Reference Files Should Have

- **Concise format** - Lists, tables, short descriptions
- **Fast scanning** - No long paragraphs
- **Common use cases** - 80% of what's needed 80% of the time
- **Links to details** - References to full documentation

## File Size Guidelines

- **CLAUDE.md**: <150 lines TARGET, <200 lines HARD LIMIT (ultra-minimal navigation index)
- **Reference docs** (docs/reference/): <300 lines (quick summaries)
- **Quick reference files**: <500 lines (fast scanning)
- **Detailed documentation**: 500-2000 lines (comprehensive coverage)
- **Large references**: 2000+ lines (split into sections if possible)

**Memory Optimization Rules**:
1. If CLAUDE.md > 150 lines → Move content to docs/reference/
2. If any summary > 50 lines → Create dedicated reference doc
3. If listing items → Create table in separate doc, link from CLAUDE.md
4. Recent work sections → Move to docs/reference/status.md

## Content Ownership

| Content Type | Location | Owned By |
|--------------|----------|----------|
| Project overview | /CLAUDE.md | AI Assistant |
| Current production tasks | /TODO.md | Development Team |
| Current architecture | docs/design/01-10-*.md | Architects |
| Future architecture | docs/design/v-next/ | Architects |
| Operational guides | docs/current/ | Development Team |
| Best practices | docs/design/best-practices.md | Architects |
| Code review findings | docs/code-review/ | AI Assistant + QA Team |
| Remediation tracking | docs/code-review/REMEDIATION-TRACKER.md | Development Team |
| User documentation | docs/user/ | Technical Writers |
| Product documentation | docs/product/ | Product Owners |

## Migration Strategy

When refactoring existing documentation:

1. **Identify** content that should move out of CLAUDE.md
2. **Create** appropriate target files in docs/current/ or docs/design/
3. **Move** content to target files
4. **Replace** with brief summary + link in CLAUDE.md
5. **Verify** target files are indexed in README.md
6. **Update** quick-reference.md with new file locations
7. **Test** that information is still accessible

## Review Schedule

### Weekly Review (During Active Development)
- Check if CLAUDE.md is still thin (<300 lines)
- Update quick-reference.md with new key files
- Verify TODO.md reflects current priorities

### Monthly Review (Ongoing Maintenance)
- Review all docs/current/ files for accuracy
- Update dependency versions in dependencies.md
- Check for outdated troubleshooting advice

### Quarterly Review (Architecture Changes)
- Review docs/design/ for accuracy vs. implementation
- Update architecture diagrams if significant changes
- Verify cross-references between documents still valid

## Implementation Notes

This protocol should be:
- **Referenced** at the start of each AI assistant session
- **Followed** when creating or updating documentation
- **Evolved** as the project and team needs change
- **Shared** with the development team for awareness

---

**Last Updated**: 2025-10-22
**Version**: 2.0
**Status**: Active

**Change Log**:
- 2025-10-22: **MAJOR UPDATE** - Aggressive memory optimization
  - Changed target from <300 to <150 lines (HARD LIMIT: <200)
  - CLAUDE.md now ultra-minimal navigation index only
  - Added docs/reference/ for summaries and overviews
  - Added memory optimization rules
  - Philosophy: "Look up when needed" not "load everything"
- 2025-10-16: Added BUILD-VALIDATION-PROTOCOL.md, PHASES-11-55-SUMMARY.md
- 2025-10-15: Added code review documentation section and context loading rules
- 2025-10-14: Initial version
