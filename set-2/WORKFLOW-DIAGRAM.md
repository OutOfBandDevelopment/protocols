# Documentation Workflow - Visual Diagram

**Visual representation of the complete documentation lifecycle**

---

## Master Workflow Overview

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     DOCUMENTATION LIFECYCLE ENTRY POINTS                    │
└─────────────────────────────────────────────────────────────────────────────┘

Entry 1: PRs Merged          Entry 2: Merge Master       Entry 3: Quality Review
    │                             │                             │
    ├─ 4+ PRs                     ├─ On docs branch           ├─ Periodic review
    ├─ Complex changes            ├─ Need master updates      ├─ Before audit
    └─ Need documentation         └─ Code changed in master   └─ Gaps suspected
         │                             │                             │
         ▼                             ▼                             ▼
   PR-DOC-UPDATE              MERGE-AND-DOC-UPDATE        GAP-ANALYSIS
   (7 phases, 2-8h)           (7 phases, 2-8h)            (7 phases, 4-200h)
         │                             │                             │
         └─────────────────┬───────────┴─────────────────────────────┘
                           │
                           ▼
         ┌─────────────────────────────────────┐
         │  ARCHITECTURE-DOCUMENTATION-MAINT   │
         │  Update C4 model docs (6 phases)    │
         │  Time: 4-20 hours                   │
         └─────────────────┬───────────────────┘
                           │
                           ▼
         ┌─────────────────────────────────────┐
         │  SOURCE-CODE-VERIFICATION           │
         │  Verify accuracy (7 phases)         │
         │  Time: 2-16 hours                   │
         └─────────────────┬───────────────────┘
                           │
                           ▼
         ┌─────────────────────────────────────┐
         │  COMPLETION                         │
         │  - Archive change tracking          │
         │  - Update TODO.md                   │
         │  - Update CLAUDE.md                 │
         └─────────────────────────────────────┘
```

---

## Detailed Phase Flow

```
┌────────────────────────────────────────────────────────────────────────────────┐
│                        PR DOCUMENTATION UPDATE WORKFLOW                        │
└────────────────────────────────────────────────────────────────────────────────┘

Phase 1: IDENTIFY MERGED PRs                    Work Tracking:
    ├─ Determine review range                   ┌─────────────────────┐
    ├─ Extract PR info                          │ TODO.md             │
    └─ Create PR inventory                      │ ├─ [ ] Current work │
         │                                       │ ├─ [x] Completed    │
         ▼                                       │ └─ [ ] Backlog      │
Phase 2: CATEGORIZE PRs                         └─────────────────────┘
    ├─ Major Features                                   │
    ├─ Security                                         │
    ├─ Performance                              ┌───────▼─────────────┐
    ├─ Bug Fixes                                │ Change Tracking     │
    ├─ Infrastructure                           │ docs/changes/       │
    ├─ Database                                 │ {topic}.md          │
    └─ Tests                                    │                     │
         │                                       │ - Objectives        │
         ▼                                       │ - Work plan         │
Phase 3: UPDATE ARCHITECTURE DOCS               │ - Changes made      │
    ├─ Map PRs to architecture docs             │ - Metrics           │
    ├─ Update each C4 document                  │ - Issues            │
    │   ├─ 01-system-context.md                 │ - Verification      │
    │   ├─ 02-containers.md                     └─────────────────────┘
    │   ├─ 03-components.md                              │
    │   ├─ 04-apis.md                                    │
    │   ├─ 05-data-model.md                              │
    │   ├─ 06-authentication-security.md                 │
    │   ├─ 07-data-synchronization.md                    │
    │   ├─ 08-configuration-management.md                │
    │   ├─ 09-deployment-architecture.md                 │
    │   └─ 10-testing-strategy.md                        │
    └─ Add source code references                        │
         │                                                │
         ▼                                                │
Phase 4: UPDATE STATUS TRACKING                          │
    ├─ Update status.md                          ────────┴────────
    ├─ Update CLAUDE.md (recent work)            Track progress
    └─ Organize by category                      throughout
         │
         ▼
Phase 5: CREATE ANALYSIS DOCUMENTS
    ├─ Create CHANGES-SINCE-{COMMIT}.md
    ├─ Executive summary
    ├─ Detailed PR analysis
    ├─ Architecture impact assessment
    └─ Next steps
         │
         ▼
Phase 6: QUALITY ASSURANCE
    ├─ Validate documentation
    ├─ Verify source code
    ├─ Check cross-references
    └─ Validate builds
         │
         ▼
Phase 7: COMMIT AND FINALIZE
    ├─ Commit architecture updates
    ├─ Commit status updates
    ├─ Commit analysis documents
    ├─ Archive change tracking ────────────────────────┐
    └─ Push changes                                     │
                                                        ▼
                                              ┌─────────────────────┐
                                              │ Archive             │
                                              │ docs/changes/       │
                                              │ archive/YYYY/QX/    │
                                              └─────────────────────┘
```

---

## Decision Tree Flow

```
┌────────────────────────────────────────────────────────────────────────────────┐
│                         PROTOCOL SELECTION DECISION TREE                       │
└────────────────────────────────────────────────────────────────────────────────┘

What happened?
    │
    ├─ PRs merged to master
    │   ├─ How many?
    │   │   ├─ 1-3 simple PRs ──────────────────────► Direct update (1-3h)
    │   │   │                                          + Change tracking
    │   │   │
    │   │   └─ 4+ PRs OR complex ────────────────────► PR-DOCUMENTATION-UPDATE
    │   │                                              (7 phases, 2-8h)
    │   │                                              + Change tracking
    │   │                                              ↓
    │   │                                          ARCHITECTURE-MAINT ───┐
    │   │                                              ↓                  │
    │   │                                          SOURCE-VERIFICATION ◄──┘
    │   │
    │   └─ Architecture changed?
    │       ├─ Minor (1-2 docs) ──────────────────────► Direct update
    │       │                                           + Change tracking
    │       │                                           ↓
    │       │                                        SOURCE-VERIFICATION
    │       │
    │       └─ Major (3+ docs) ───────────────────────► ARCHITECTURE-MAINT
    │                                                   (6 phases, 4-20h)
    │                                                   + Change tracking
    │                                                   ↓
    │                                               SOURCE-VERIFICATION
    │
    ├─ On documentation branch, master has changes ───► MERGE-AND-DOC-UPDATE
    │                                                   (7 phases, 2-8h)
    │                                                   + Change tracking
    │                                                   ↓
    │                                               PR-DOCUMENTATION-UPDATE
    │                                                   ↓
    │                                               ARCHITECTURE-MAINT
    │                                                   ↓
    │                                               SOURCE-VERIFICATION
    │
    ├─ Documentation quality concerns
    │   ├─ Accuracy questioned ───────────────────────► SOURCE-VERIFICATION
    │   │                                              (7 phases, 2-16h)
    │   │                                              + Change tracking
    │   │                                              ↓
    │   │                                           Fix errors & commit
    │   │
    │   ├─ Gaps suspected ────────────────────────────► GAP-ANALYSIS
    │   │                                              (7 phases, 4-200h)
    │   │                                              + Change tracking
    │   │                                              ↓
    │   │                                           Remediation plan
    │   │                                              ↓
    │   │                                           Execute fixes ────────┐
    │   │                                              ↓                  │
    │   │                                           SOURCE-VERIFICATION ◄──┘
    │   │
    │   └─ Documentation out of date ──────────────────► Determine cause
    │                                                      ├─ PRs? → PR-DOC-UPDATE
    │                                                      ├─ Arch? → ARCHITECTURE-MAINT
    │                                                      └─ Unknown? → GAP-ANALYSIS
    │
    └─ New user-facing feature ───────────────────────► USER-GUIDE-GENERATION
                                                         (varies, 8-40h)
                                                         + Change tracking
                                                         ↓
                                                     SOURCE-VERIFICATION

All paths end with:
    ↓
┌───────────────────────────────────────┐
│ COMPLETION CHECKLIST                  │
│ ─────────────────────────────────────│
│ □ Documentation committed             │
│ □ Change tracking archived            │
│ □ TODO.md updated                     │
│ □ CLAUDE.md updated                   │
│ □ Build validated (if needed)         │
│ □ No TODOs in docs                    │
│ □ No broken links                     │
└───────────────────────────────────────┘
```

---

## Work Tracking Flow

```
┌────────────────────────────────────────────────────────────────────────────────┐
│                           WORK TRACKING LIFECYCLE                              │
└────────────────────────────────────────────────────────────────────────────────┘

START NEW WORK
    │
    ├─ Update TODO.md
    │   └─ Add task with:
    │       ├─ Description
    │       ├─ Protocol to use
    │       ├─ Change tracking file name
    │       ├─ Estimated effort
    │       └─ Due date
    │
    ├─ Create change tracking document
    │   └─ docs/changes/{major-topic}-{minor-topic}-{description}.md
    │       ├─ Objective
    │       ├─ Work plan (phases)
    │       ├─ Changes made (updated as you work)
    │       ├─ Metrics
    │       └─ Issues & decisions
    │
    └─ Execute protocol
         │
         ▼
DURING WORK (update frequently)
    │
    ├─ Update TODO.md status
    │   ├─ Pending → In Progress
    │   ├─ Update % complete
    │   └─ Note blockers
    │
    └─ Update change tracking
        ├─ Mark phases complete
        ├─ Document changes made
        ├─ Track metrics
        └─ Record issues/decisions
         │
         ▼
COMPLETE WORK
    │
    ├─ Final change tracking update
    │   ├─ Completion date
    │   ├─ Final metrics
    │   ├─ Lessons learned
    │   └─ Next steps
    │
    ├─ Archive change tracking
    │   └─ Move to: docs/changes/archive/YYYY/QX/{file}.md
    │
    ├─ Update TODO.md
    │   └─ Move task to "Completed This Sprint"
    │
    └─ Update CLAUDE.md
        └─ Add to "Recent Session Work"


File Organization:

docs/changes/                          docs/changes/archive/
├── docs-prs-current.md                ├── 2026/
├── architecture-update.md             │   ├── Q1/
├── verify-apis.md                     │   │   ├── docs-prs-batch-1.md
└── .gitkeep                           │   │   ├── gap-analysis-q1.md
                                       │   │   └── merge-master-2026-01-02.md
    (active work)                      │   └── Q2/
                                       └── 2025/
                                           └── Q4/

                                       (completed work, kept for reference)
```

---

## Quality Gate Flow

```
┌────────────────────────────────────────────────────────────────────────────────┐
│                              QUALITY GATES                                     │
└────────────────────────────────────────────────────────────────────────────────┘

GATE 1: Before Committing Documentation
    ├─ □ All identified changes documented
    ├─ □ Source code verification completed (minimum: spot-check)
    ├─ □ Code examples tested (if applicable)
    ├─ □ Cross-references validated
    ├─ □ TODO.md updated
    └─ □ Change tracking updated
         │
         ├─ PASS → Commit documentation
         │          ↓
         └─ FAIL → Fix issues, retry gate
                    ↓
GATE 2: Before Merging to Master
    ├─ □ All documentation changes committed
    ├─ □ Source code verification comprehensive
    ├─ □ No broken links
    ├─ □ No TODO/FIXME markers in docs
    ├─ □ CLAUDE.md updated
    └─ □ Change tracking archived
         │
         ├─ PASS → Create PR, merge to master
         │          ↓
         └─ FAIL → Fix issues, retry gate
                    ↓
GATE 3: Periodic Quality Reviews
    │
    ├─ QUARTERLY
    │   ├─ □ Gap analysis completed
    │   ├─ □ Coverage metrics calculated
    │   └─ □ Remediation plan created
    │
    └─ SEMI-ANNUAL
        ├─ □ Meta-review completed
        ├─ □ All architecture docs verified
        └─ □ All code examples tested

         ├─ PASS → Documentation healthy
         │          ↓
         │        Continue normal workflow
         │
         └─ FAIL → Create remediation plan
                    ↓
                 Execute remediation
                    ↓
                 Re-check quality
```

---

## Integration Map

```
┌────────────────────────────────────────────────────────────────────────────────┐
│                         PROTOCOL INTEGRATION MAP                               │
└────────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────┐
│ MEMORY-PROTOCOL          │ ◄────── Load at every AI session start
└────────────┬─────────────┘
             │
             ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│ DOCUMENTATION-WORKFLOW-LIFECYCLE (Master Workflow)                           │
│ - Determine which protocol to use                                            │
│ - Set up work tracking                                                       │
└────────────┬─────────────────────────────────────────────────────────────────┘
             │
             ├─────────────┬─────────────┬─────────────┬─────────────┬─────────┐
             ▼             ▼             ▼             ▼             ▼         ▼
    ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐
    │ PR-DOC-    │  │ ARCH-DOC-  │  │ GAP-       │  │ SOURCE-    │  │ USER-GUIDE │
    │ UPDATE     │  │ MAINT      │  │ ANALYSIS   │  │ VERIF      │  │ GENERATION │
    └──────┬─────┘  └──────┬─────┘  └──────┬─────┘  └──────┬─────┘  └──────┬─────┘
           │               │               │               │               │
           │               │               ├───────────────┤               │
           │               │               │                               │
           ├───────────────┴───────────────┤                               │
           │                               │                               │
           │                               │                               │
           │                               ▼                               │
           │                    ┌─────────────────────┐                    │
           │                    │ ARCHITECTURE-DOC-   │                    │
           │                    │ MAINTENANCE         │                    │
           │                    │ (if arch changed)   │                    │
           │                    └──────────┬──────────┘                    │
           │                               │                               │
           └───────────────────────────────┴───────────────────────────────┘
                                           │
                                           ▼
                                ┌─────────────────────┐
                                │ SOURCE-CODE-        │
                                │ VERIFICATION        │
                                │ (always verify)     │
                                └──────────┬──────────┘
                                           │
                ┌──────────────────────────┴──────────────────────────┐
                │                                                     │
                ▼                                                     ▼
    ┌──────────────────────┐                           ┌──────────────────────┐
    │ BUILD-VALIDATION     │                           │ Update TODO.md       │
    │ (if needed)          │                           │ Archive tracking     │
    └──────────────────────┘                           │ Update CLAUDE.md     │
                                                       └──────────────────────┘
                                                                 │
                                                                 ▼
                                                       ┌──────────────────────┐
                                                       │ COMPLETE             │
                                                       └──────────────────────┘

Supporting Protocols (periodic):
┌──────────────────────┐  ┌──────────────────────┐  ┌──────────────────────┐
│ CODE-REVIEW-         │  │ META-REVIEW-         │  │ BUILD-METRICS-       │
│ PROTOCOL             │  │ PROTOCOL             │  │ PROTOCOL             │
│ (code review work)   │  │ (semi-annual)        │  │ (periodic)           │
└──────────────────────┘  └──────────────────────┘  └──────────────────────┘
```

---

## File Lifecycle

```
┌────────────────────────────────────────────────────────────────────────────────┐
│                            DOCUMENTATION FILE LIFECYCLE                        │
└────────────────────────────────────────────────────────────────────────────────┘

CODE CHANGES (PRs merged, architecture evolves)
    │
    ▼
PLANNING
    ├─ Create: docs/changes/{topic}.md (change tracking)
    └─ Update: TODO.md (add task)
    │
    ▼
DOCUMENTATION UPDATES
    ├─ Update: docs/design/*.md (architecture docs)
    ├─ Update: docs/reference/status.md (status tracking)
    ├─ Create: docs/reference/CHANGES-SINCE-{COMMIT}.md (PR analysis)
    └─ Update: docs/user-guide/*.md (user guides, if needed)
    │
    ▼
VERIFICATION
    ├─ Create: .verification/*.md (verification reports)
    └─ Fix errors in documentation
    │
    ▼
COMMIT
    ├─ Commit: All updated documentation
    ├─ Commit message: References change tracking & PRs
    └─ Tag with source verification note
    │
    ▼
COMPLETION
    ├─ Archive: docs/changes/{topic}.md → docs/changes/archive/YYYY/QX/
    ├─ Update: TODO.md (mark complete)
    ├─ Update: CLAUDE.md (recent session work)
    └─ Clean up: .verification/ files
    │
    ▼
MAINTENANCE
    ├─ Periodic verification (monthly/quarterly)
    ├─ Gap analysis (quarterly)
    └─ Meta-review (semi-annual)
```

---

## Legend

```
Symbols:
    │   Flow direction
    ├─  Branch point
    └─  End of branch
    ▼   Process continues downward
    ►   Direct to
    ◄─  Feedback loop
    □   Checkbox (unchecked)
    ☑   Checkbox (checked)

Protocols:
    PR-DOC-UPDATE         PR-DOCUMENTATION-UPDATE-PROTOCOL
    ARCH-DOC-MAINT        ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL
    GAP-ANALYSIS          DOCUMENTATION-GAP-ANALYSIS-PROTOCOL
    SOURCE-VERIF          SOURCE-CODE-VERIFICATION-PROTOCOL
    USER-GUIDE            USER-GUIDE-GENERATION-PROTOCOL
    MERGE-AND-DOC         MERGE-AND-DOC-UPDATE-PROTOCOL
    BUILD-VALIDATION      BUILD-VALIDATION-PROTOCOL
    BUILD-METRICS         BUILD-METRICS-PROTOCOL
    CODE-REVIEW           CODE-REVIEW-PROTOCOL
    META-REVIEW           META-REVIEW-PROTOCOL
    MEMORY                MEMORY-PROTOCOL

Time Estimates:
    Quick:    < 2 hours
    Short:    2-8 hours
    Medium:   8-40 hours
    Long:     40-200 hours
```

---

**For AI Assistants**: Use these diagrams to:
- Visualize the complete workflow
- Understand protocol relationships
- Follow decision trees
- Track work through the lifecycle

**For Developers**: Use these diagrams to:
- Understand documentation workflow
- Know when to use which protocol
- Track work progress
- Maintain documentation quality

---

**Last Updated**: 2026-01-03
**See Also**: [DOCUMENTATION-WORKFLOW-LIFECYCLE.md](DOCUMENTATION-WORKFLOW-LIFECYCLE.md)
