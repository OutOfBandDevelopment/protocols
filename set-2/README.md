# CadLink Documentation Protocols

**Purpose**: Standardized protocols for maintaining high-quality documentation throughout the development lifecycle.

**Last Updated**: 2026-01-03

---

## Quick Start

**New to these protocols?** Start here:
1. Read **[DOCUMENTATION-WORKFLOW-LIFECYCLE.md](DOCUMENTATION-WORKFLOW-LIFECYCLE.md)** - The master workflow
2. Identify your scenario (PRs merged? Architecture changed? Quality review?)
3. Follow the decision tree to find the right protocol
4. Create a change tracking document in `docs/changes/`
5. Update `TODO.md` with your work
6. Execute the protocol
7. Archive change tracking when complete

---

## Protocol Overview

### 🌟 Master Workflow

**[DOCUMENTATION-WORKFLOW-LIFECYCLE.md](DOCUMENTATION-WORKFLOW-LIFECYCLE.md)**
- **Purpose**: Shows how all protocols integrate into a cohesive lifecycle
- **When to use**: Start of any documentation work to determine the right path
- **Key features**:
  - Decision trees for protocol selection
  - Work tracking mechanisms (TODO.md + change tracking)
  - 4 complete workflow examples
  - Quality gates and best practices
  - Emergency procedures

---

## Protocol Categories

### Core Protocols

#### [MEMORY-PROTOCOL.md](MEMORY-PROTOCOL.md)
- **Purpose**: AI assistant memory management and context loading
- **When to use**: Every AI session start
- **Key features**: Context loading rules, memory constraints

#### [CODE-REVIEW-PROTOCOL.md](CODE-REVIEW-PROTOCOL.md)
- **Purpose**: Systematic code review process
- **When to use**: Code review activities, remediation tracking
- **Phases**: Multiple (comprehensive code review framework)

#### [META-REVIEW-PROTOCOL.md](META-REVIEW-PROTOCOL.md)
- **Purpose**: Review of review processes and protocols
- **When to use**: Periodic protocol effectiveness reviews
- **Frequency**: Semi-annual

---

### Documentation Protocols

#### [PR-DOCUMENTATION-UPDATE-PROTOCOL.md](PR-DOCUMENTATION-UPDATE-PROTOCOL.md)
- **Purpose**: Document merged Pull Requests systematically
- **When to use**: 4+ PRs merged OR complex PR changes
- **Phases**: 7 (Identify → Categorize → Update Architecture → Update Status → Analysis → QA → Commit)
- **Time**: 2-8 hours
- **Outputs**: Updated architecture docs, status.md, CHANGES-SINCE-{COMMIT}.md

#### [DOCUMENTATION-GAP-ANALYSIS-PROTOCOL.md](DOCUMENTATION-GAP-ANALYSIS-PROTOCOL.md)
- **Purpose**: Identify and close documentation gaps
- **When to use**: Periodic reviews, before audits, quality concerns
- **Phases**: 7 (Scope → Identify → Measure → Prioritize → Plan → Execute → Validate)
- **Time**: 4-16h analysis, 40-200h remediation (varies by gap count)
- **Outputs**: Gap metrics, remediation plan, completion report

#### [ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL.md](ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL.md)
- **Purpose**: Keep C4 model documentation in sync with code
- **When to use**: After code changes affecting architecture
- **Phases**: 6 (Identify Changes → Update C4 Docs → Cross-Ref → Validate → ADRs → Commit)
- **Time**: 4-20 hours
- **Outputs**: Updated C4 docs (01-10), ADRs

#### [SOURCE-CODE-VERIFICATION-PROTOCOL.md](SOURCE-CODE-VERIFICATION-PROTOCOL.md)
- **Purpose**: Verify documentation accuracy against source code
- **When to use**: After updating docs, before audits, when accuracy questioned
- **Phases**: 7 (Scope → Verify Entities → Verify APIs → Verify Config → Verify Examples → Report → Correct)
- **Time**: 2-8h verification, 2-8h corrections
- **Outputs**: Verification report, corrected documentation

#### [USER-GUIDE-GENERATION-PROTOCOL.md](USER-GUIDE-GENERATION-PROTOCOL.md)
- **Purpose**: Create user-facing documentation
- **When to use**: New user-facing features, user documentation needs
- **Time**: 8-40 hours (varies by feature complexity)
- **Outputs**: User guides with screenshots, workflows, troubleshooting

---

### Merge & Build Protocols

#### [MERGE-AND-DOC-UPDATE-PROTOCOL.md](MERGE-AND-DOC-UPDATE-PROTOCOL.md)
- **Purpose**: Merge master into documentation branch and update docs
- **When to use**: On docs branch, need to merge master
- **Phases**: 7 (Prep → Merge → Analyze → Update → QA → Commit → Push)
- **Time**: 2-8 hours
- **Outputs**: Merged branch, updated docs, merge summary
- **Script**: `execute-merge-protocol.ps1` (automated execution)

#### [BUILD-VALIDATION-PROTOCOL.md](BUILD-VALIDATION-PROTOCOL.md)
- **Purpose**: Validate build after changes
- **When to use**: After significant changes, before release
- **Time**: 30 minutes - 2 hours
- **Outputs**: Build validation report

#### [BUILD-METRICS-PROTOCOL.md](BUILD-METRICS-PROTOCOL.md)
- **Purpose**: Track and analyze build metrics
- **When to use**: Periodic build performance reviews
- **Time**: 1-3 hours
- **Outputs**: Build metrics report, trends

---

## Protocol Selection Guide

| Scenario | Protocol | Change Tracking Name |
|----------|----------|----------------------|
| **4+ PRs merged** | PR-DOCUMENTATION-UPDATE | `docs-prs-{batch}` |
| **1-3 PRs merged** | Direct update | `docs-prs-{description}` |
| **Architecture changed** | ARCHITECTURE-MAINTENANCE | `architecture-{topic}` |
| **Accuracy questioned** | SOURCE-CODE-VERIFICATION | `verify-{topic}` |
| **Quality review** | DOCUMENTATION-GAP-ANALYSIS | `gap-analysis-{quarter}` |
| **Merge master** | MERGE-AND-DOC-UPDATE | `merge-master-{date}` |
| **New user guide** | USER-GUIDE-GENERATION | `user-guide-{feature}` |

See **[DOCUMENTATION-WORKFLOW-LIFECYCLE.md](DOCUMENTATION-WORKFLOW-LIFECYCLE.md)** for complete decision trees.

---

## Work Tracking Structure

### Primary Tracking: `./TODO.md`

Track active work, blockers, completed work, backlog:
```markdown
## Current Work

- [ ] Document PRs 13084-13095 (12 PRs)
  - Status: 60% complete
  - Protocol: PR-DOCUMENTATION-UPDATE-PROTOCOL
  - Change Tracking: docs/changes/docs-prs-week-of-2026-01-06.md
  - Due: 2026-01-10

## Completed This Sprint
- [x] Merge master into docs branch - 2026-01-02
  - Change Tracking: docs/changes/merge-master-2026-01-02.md
```

### Change Tracking: `./docs/changes/{topic}.md`

Detailed tracking for each documentation effort:
```markdown
# Change Tracking: Document PRs Week of 2026-01-06

**Protocol**: PR-DOCUMENTATION-UPDATE-PROTOCOL
**Status**: In Progress

## Work Plan
- [x] Phase 1: Identify PRs
- [x] Phase 2: Categorize
- [ ] Phase 3: Update architecture (in progress)
```

**Naming Convention**: `{major-topic}-{minor-topic}-{description}.md`
- Examples: `docs-prs-batch-5.md`, `architecture-security-update.md`, `verify-api-docs.md`

**Archive When Complete**: Move to `docs/changes/archive/YYYY/QX/`

---

## Typical Workflows

### Workflow 1: Weekly PR Documentation

```
Monday: Plan
  ├─ Update TODO.md with task
  ├─ Create docs/changes/docs-prs-week-{date}.md
  └─ Run PR-DOCUMENTATION-UPDATE-PROTOCOL Phase 1-2

Tuesday-Wednesday: Execute
  ├─ PR-DOCUMENTATION-UPDATE-PROTOCOL Phase 3-4
  └─ Update architecture docs

Thursday: Verify
  ├─ SOURCE-CODE-VERIFICATION-PROTOCOL (quick pass)
  └─ Fix errors

Friday: Complete
  ├─ PR-DOCUMENTATION-UPDATE-PROTOCOL Phase 7
  ├─ Archive change tracking
  ├─ Update TODO.md as complete
  └─ Update CLAUDE.md
```

### Workflow 2: Quarterly Gap Analysis

```
Week 1: Analyze
  ├─ DOCUMENTATION-GAP-ANALYSIS-PROTOCOL Phase 1-4
  ├─ Create docs/changes/gap-analysis-q1-2026.md
  └─ Create remediation plan

Week 2-3: Remediate
  ├─ DOCUMENTATION-GAP-ANALYSIS-PROTOCOL Phase 6
  ├─ Update docs (use ARCHITECTURE-MAINTENANCE as needed)
  └─ Verify with SOURCE-CODE-VERIFICATION

Week 4: Validate
  ├─ DOCUMENTATION-GAP-ANALYSIS-PROTOCOL Phase 7
  ├─ Create completion report
  └─ Archive change tracking
```

### Workflow 3: Architecture Change

```
Identify Change
  ├─ Create docs/changes/architecture-{component}.md
  └─ Update TODO.md

Update Documentation
  ├─ ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL
  └─ Update C4 docs

Verify Accuracy
  ├─ SOURCE-CODE-VERIFICATION-PROTOCOL
  └─ Fix errors

Complete
  ├─ Archive change tracking
  └─ Update CLAUDE.md
```

---

## Quality Gates

### Before Commit
- [ ] Documentation updated for all changes
- [ ] Source verification completed (spot-check minimum)
- [ ] Code examples tested (if applicable)
- [ ] TODO.md updated
- [ ] Change tracking updated

### Before Merge to Master
- [ ] All documentation committed
- [ ] Source verification comprehensive
- [ ] No broken links
- [ ] No TODO/FIXME markers
- [ ] CLAUDE.md updated
- [ ] Change tracking archived

### Periodic Reviews
- [ ] Quarterly: Gap analysis
- [ ] Semi-annual: Meta-review
- [ ] Monthly: Source verification

---

## Best Practices

### ✅ Do
- Create change tracking for every documentation effort
- Update TODO.md daily
- Follow protocol sequences (Document → Verify → Commit)
- Archive completed change tracking (don't delete)
- Verify against source code early and often
- Use appropriate protocol for task size

### ❌ Don't
- Skip change tracking for "small" tasks
- Let TODO.md go stale (> 3 days without update)
- Commit documentation without verification
- Delete change tracking docs
- Skip protocols for large tasks (> 8 hours)
- Mix completed and active work in TODO.md

---

## Protocol Integration Flow

```
Entry Point
    ├─ PRs merged → PR-DOCUMENTATION-UPDATE-PROTOCOL
    ├─ Architecture changed → ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL
    ├─ Quality review → DOCUMENTATION-GAP-ANALYSIS-PROTOCOL
    └─ Accuracy questioned → SOURCE-CODE-VERIFICATION-PROTOCOL

All paths converge to:
    ↓
SOURCE-CODE-VERIFICATION-PROTOCOL (verify accuracy)
    ↓
Update TODO.md & Archive Change Tracking
    ↓
Update CLAUDE.md Recent Session Work
    ↓
Complete
```

---

## File Structure

```
CadLink/
├── .claude/
│   └── protocols/
│       ├── README.md (this file)
│       ├── DOCUMENTATION-WORKFLOW-LIFECYCLE.md (master workflow)
│       ├── PR-DOCUMENTATION-UPDATE-PROTOCOL.md
│       ├── DOCUMENTATION-GAP-ANALYSIS-PROTOCOL.md
│       ├── ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL.md
│       ├── SOURCE-CODE-VERIFICATION-PROTOCOL.md
│       ├── USER-GUIDE-GENERATION-PROTOCOL.md
│       ├── MERGE-AND-DOC-UPDATE-PROTOCOL.md
│       ├── BUILD-VALIDATION-PROTOCOL.md
│       ├── BUILD-METRICS-PROTOCOL.md
│       ├── CODE-REVIEW-PROTOCOL.md
│       ├── META-REVIEW-PROTOCOL.md
│       ├── MEMORY-PROTOCOL.md
│       └── execute-merge-protocol.ps1
│
├── TODO.md (primary work tracking)
│
├── docs/
│   ├── changes/ (change tracking)
│   │   ├── .gitkeep
│   │   ├── [active tracking docs]
│   │   └── archive/
│   │       ├── 2026/
│   │       │   └── Q1/
│   │       └── 2025/
│   │           └── Q4/
│   └── [other documentation...]
│
└── CLAUDE.md (project overview)
```

---

## Getting Help

### For AI Assistants
1. Load **[MEMORY-PROTOCOL.md](MEMORY-PROTOCOL.md)** at session start
2. Reference **[DOCUMENTATION-WORKFLOW-LIFECYCLE.md](DOCUMENTATION-WORKFLOW-LIFECYCLE.md)** for protocol selection
3. Follow selected protocol step-by-step
4. Use TodoWrite tool to track progress
5. Create change tracking documents
6. Update TODO.md throughout work
7. Update CLAUDE.md at session end

### For Developers
1. Review **[DOCUMENTATION-WORKFLOW-LIFECYCLE.md](DOCUMENTATION-WORKFLOW-LIFECYCLE.md)**
2. Identify your scenario
3. Follow the recommended protocol
4. Track work in TODO.md and change tracking
5. Follow quality gates before committing

---

## Version History

| Version | Date | Changes | Author |
|---------|------|---------|--------|
| 1.0 | 2026-01-03 | Initial protocol suite creation | Claude |

---

## Contributing

To add or modify protocols:
1. Follow existing protocol format (Purpose, Overview, Phases, etc.)
2. Update this README with new protocol
3. Update DOCUMENTATION-WORKFLOW-LIFECYCLE.md decision trees
4. Update CLAUDE.md protocol list
5. Test protocol on real documentation work
6. Document lessons learned

---

## Quick Reference Card

```bash
# Start new work
touch docs/changes/{topic}.md
# Update TODO.md with task

# Execute protocol
# [Follow protocol phases]

# Complete work
mv docs/changes/{doc}.md docs/changes/archive/$(date +%Y)/Q{quarter}/
# Update TODO.md as complete
# Update CLAUDE.md recent session work
```

**Common Change Tracking Names**:
- `docs-prs-{batch}` - PR documentation
- `architecture-{topic}` - Architecture updates
- `gap-analysis-{quarter}` - Gap analysis
- `verify-{topic}` - Verification work
- `merge-master-{date}` - Master merges
- `user-guide-{feature}` - User guides

**Protocol Time Estimates**:
- PR Documentation: 2-8h
- Architecture Maintenance: 4-20h
- Gap Analysis: 4-200h (varies greatly)
- Source Verification: 2-16h
- Merge Master: 2-8h

---

**Last Updated**: 2026-01-03
**Total Protocols**: 13
**Master Workflow**: DOCUMENTATION-WORKFLOW-LIFECYCLE.md
