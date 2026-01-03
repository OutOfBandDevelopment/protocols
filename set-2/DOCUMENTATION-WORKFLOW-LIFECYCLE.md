# Documentation Workflow Lifecycle

**Purpose**: Master workflow showing how all protocols integrate into a cohesive documentation lifecycle.

**Version**: 1.0
**Last Updated**: 2026-01-03
**Audience**: AI assistants, documentation maintainers, technical writers

---

## Overview

This protocol provides the **master workflow** for managing documentation throughout the development lifecycle. It shows:
1. **When to use each protocol**
2. **How protocols integrate with each other**
3. **Work tracking mechanisms** (TODO.md, change tracking)
4. **Decision trees** for different scenarios
5. **Quality gates** and validation points

**Key Concept**: Documentation isn't a one-time task—it's a continuous lifecycle that evolves with the codebase.

---

## Workflow Entry Points

### Entry Point 1: PRs Merged to Master
**Trigger**: Pull requests have been merged, documentation needs updating
**Protocol**: [PR-DOCUMENTATION-UPDATE-PROTOCOL](PR-DOCUMENTATION-UPDATE-PROTOCOL.md)
**Next Steps**: → Architecture Maintenance → Source Verification

### Entry Point 2: On Documentation Branch, Need to Merge Master
**Trigger**: Working on docs branch, master has changes
**Protocol**: [MERGE-AND-DOC-UPDATE-PROTOCOL](MERGE-AND-DOC-UPDATE-PROTOCOL.md)
**Next Steps**: → PR Documentation Update → Architecture Maintenance

### Entry Point 3: Documentation Quality Review
**Trigger**: Periodic review, preparing for audit, onboarding new team
**Protocol**: [DOCUMENTATION-GAP-ANALYSIS-PROTOCOL](DOCUMENTATION-GAP-ANALYSIS-PROTOCOL.md)
**Next Steps**: → Gap Remediation → Source Verification

### Entry Point 4: Code Changes Made
**Trigger**: You made code changes that affect architecture
**Protocol**: [ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL](ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL.md)
**Next Steps**: → Source Verification → Update TODO.md

### Entry Point 5: Documentation Accuracy Questioned
**Trigger**: Found error in docs, need to verify accuracy
**Protocol**: [SOURCE-CODE-VERIFICATION-PROTOCOL](SOURCE-CODE-VERIFICATION-PROTOCOL.md)
**Next Steps**: → Correct Errors → Update TODO.md

### Entry Point 6: New Feature Needs User Guide
**Trigger**: User-facing feature added, needs documentation
**Protocol**: [USER-GUIDE-GENERATION-PROTOCOL](USER-GUIDE-GENERATION-PROTOCOL.md)
**Next Steps**: → Source Verification

---

## Core Workflow: The Documentation Lifecycle

```
┌─────────────────────────────────────────────────────────────────┐
│                      DOCUMENTATION LIFECYCLE                     │
└─────────────────────────────────────────────────────────────────┘

Phase 1: PLANNING & TRACKING
    ├─ Review ./TODO.md
    ├─ Create change tracking: ./docs/changes/{topic}.md
    ├─ Identify documentation scope
    └─ Estimate effort

Phase 2: CODE/PR CHANGES
    ├─ PRs merged to master → PR-DOCUMENTATION-UPDATE-PROTOCOL
    ├─ Code changes made → Track in change document
    └─ Architecture evolves → Note for architecture update

Phase 3: DOCUMENTATION UPDATE
    ├─ Update architecture docs → ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL
    ├─ Document PRs → PR-DOCUMENTATION-UPDATE-PROTOCOL
    ├─ Create user guides → USER-GUIDE-GENERATION-PROTOCOL
    └─ Update change tracking document

Phase 4: VERIFICATION & VALIDATION
    ├─ Verify accuracy → SOURCE-CODE-VERIFICATION-PROTOCOL
    ├─ Validate build → BUILD-VALIDATION-PROTOCOL
    ├─ Check coverage → DOCUMENTATION-GAP-ANALYSIS-PROTOCOL (periodic)
    └─ Update TODO.md with findings

Phase 5: QUALITY ASSURANCE
    ├─ Code review → CODE-REVIEW-PROTOCOL (if applicable)
    ├─ Meta-review → META-REVIEW-PROTOCOL (periodic)
    └─ Fix identified issues

Phase 6: INTEGRATION & COMPLETION
    ├─ Merge documentation changes
    ├─ Archive change tracking document
    ├─ Update ./TODO.md status
    └─ Update CLAUDE.md recent session work

Phase 7: MAINTENANCE
    ├─ Monitor for documentation drift
    ├─ Schedule next gap analysis
    └─ Plan next documentation sprint
```

---

## Work Tracking Structure

### 1. Primary Work Tracking: ./TODO.md

**Purpose**: Track current work, active tasks, blockers

**Format**:
```markdown
# TODO - CadLink Documentation

**Last Updated**: [YYYY-MM-DD]
**Active Sprint**: [Sprint Name/Date Range]

---

## Current Work

### In Progress

- [ ] **Document PRs 13084-13095** (12 PRs)
  - Status: 60% complete (7/12 PRs documented)
  - Protocol: PR-DOCUMENTATION-UPDATE-PROTOCOL
  - Change Tracking: `docs/changes/docs-prs-december-batch-2.md`
  - Assigned: [Name]
  - Due: 2026-01-10
  - Blockers: None

- [ ] **Verify Architecture Documentation Accuracy**
  - Status: Planning
  - Protocol: SOURCE-CODE-VERIFICATION-PROTOCOL
  - Change Tracking: `docs/changes/verify-architecture-accuracy.md`
  - Assigned: [Name]
  - Due: 2026-01-15
  - Blockers: Waiting for PR documentation to complete

### Pending (Not Started)

- [ ] **Gap Analysis - Q1 2026**
  - Status: Scheduled
  - Protocol: DOCUMENTATION-GAP-ANALYSIS-PROTOCOL
  - Scheduled: 2026-02-01
  - Estimated: 40h (8h analysis + 32h remediation)

### Blocked

- [ ] **User Guide for Stream Tracking**
  - Status: Blocked
  - Protocol: USER-GUIDE-GENERATION-PROTOCOL
  - Blocker: Waiting for UI screenshots from design team
  - Assigned: [Name]

---

## Completed This Sprint

- [x] **Merge master into docs branch** - 2026-01-02
  - Protocol: MERGE-AND-DOC-UPDATE-PROTOCOL
  - Change Tracking: `docs/changes/merge-master-2026-01-02.md`
  - Result: 45 commits merged, 8 docs updated

- [x] **Document PRs 13070-13083** - 2026-01-01
  - Protocol: PR-DOCUMENTATION-UPDATE-PROTOCOL
  - Change Tracking: `docs/changes/docs-prs-december-batch-1.md`
  - Result: 14 PRs documented, 5 architecture docs updated

---

## Backlog (Future Work)

- [ ] Update deployment documentation (low priority)
- [ ] Create troubleshooting guide expansion
- [ ] API documentation for minor services

---

## Maintenance Schedule

- **Gap Analysis**: Quarterly (Next: 2026-04-01)
- **Source Verification**: Monthly (Next: 2026-02-01)
- **Meta-Review**: Semi-annual (Next: 2026-06-01)
- **Build Validation**: Per protocol as needed

---

## Notes

- Focusing on PR documentation backlog this sprint
- Architecture verification scheduled after PR backlog complete
- Gap analysis delayed to Q1 2026 (resource availability)
```

### 2. Change Tracking: ./docs/changes/{topic}.md

**Purpose**: Track detailed changes for specific documentation efforts

**Naming Convention**:
```
./docs/changes/{major-topic}-{minor-topic}-{description}.md

Examples:
- ./docs/changes/docs-prs-december-batch-1.md
- ./docs/changes/architecture-security-password-hashing.md
- ./docs/changes/gap-analysis-q4-2025.md
- ./docs/changes/verify-api-documentation.md
- ./docs/changes/merge-master-2026-01-02.md
- ./docs/changes/user-guide-stream-tracking.md
```

**Major Topics**:
- `docs` - Documentation updates (general)
- `architecture` - Architecture documentation changes
- `gap-analysis` - Gap analysis and remediation
- `verify` - Verification activities
- `merge` - Merge activities
- `user-guide` - User guide creation
- `api` - API documentation
- `security` - Security documentation

**Format**:
```markdown
# Change Tracking: {Description}

**Created**: [YYYY-MM-DD]
**Protocol**: [Protocol Name]
**Status**: In Progress / Complete / Blocked
**Owner**: [Name]
**Related TODO Item**: [Link to TODO.md section]

**Git Tracking**:
- **Last Doc Update**: [commit-hash] (date documentation was last updated)
- **Current Master**: [commit-hash] (master or main branch at start of this work)
- **Target Master**: [commit-hash] (master or main branch at completion of this work)

---

## Objective

[1-2 sentences describing what this change tracking document covers]

**Scope**: [What's included]
**Out of Scope**: [What's not included]
**Success Criteria**: [How to know when done]

---

## Work Plan

### Phase 1: [Phase Name]
**Estimated Effort**: [X]h
**Status**: ✅ Complete / 🔄 In Progress / 🔴 Pending / ⚠️ Blocked

**Tasks**:
- [x] Task 1 - Completed [date]
- [x] Task 2 - Completed [date]
- [ ] Task 3 - In progress

**Notes**:
- [Any notes, issues, decisions]

### Phase 2: [Phase Name]
[Repeat structure...]

---

## Changes Made

### Documentation Files Updated

#### docs/design/05-data-model.md
**Changes**:
- Added OpenStreamActivity entity (lines 145-178)
- Updated Patient entity relationships (line 87)
- Added ER diagram for stream tracking (line 200)

**Verification**: ✅ Verified against `/current/src/Source/Database/Tables/OpenStreamActivity.cs:15` on [date]

#### docs/design/06-authentication-security.md
**Changes**:
- Updated password hashing section (lines 45-89)
- Added PBKDF2 implementation details
- Updated security best practices

**Verification**: ✅ Verified against `/current/src/Source/Auth/PasswordHasher.cs:45` on [date]

[Continue for all files...]

---

## Metrics

**Documentation Files**:
- Updated: [X] files
- Created: [Y] files
- Deleted: [Z] files

**Effort**:
- Estimated: [X] hours
- Actual: [Y] hours
- Variance: [+/-Z] hours ([+/-percentage]%)

**Coverage**:
- Before: [X]%
- After: [Y]%
- Improvement: [+Z]%

---

## Issues & Decisions

### Issue 1: [Description]
**Status**: Resolved / Open
**Impact**: [High/Medium/Low]
**Resolution**: [How resolved or plan to resolve]
**Decided By**: [Name/Team]
**Date**: [YYYY-MM-DD]

[Continue for all issues...]

---

## Verification & Validation

- [x] Documentation accuracy verified against source code
- [x] Code examples tested and compile
- [x] Cross-references validated
- [x] Spelling/grammar checked
- [x] Peer reviewed
- [ ] Stakeholder approved

---

## Completion

**Completed**: [YYYY-MM-DD]
**Final Status**: Success / Partial / Failed
**Outcome**: [Brief description of outcome]

**Final Git State**:
- **Documentation Updated Against**: [commit-hash on master (or main) branch]
- **Commits Documented**: [start-hash]...[end-hash] (X commits)
- **Command to Review Changes**: `git diff [last-doc-hash]...[current-hash]`

**Lessons Learned**:
- [Lesson 1]
- [Lesson 2]

**Next Steps**:
- [ ] Archive this document
- [ ] Update TODO.md
- [ ] Update CLAUDE.md recent session work
- [ ] Schedule follow-up (if needed)

---

**Document Owner**: [Name]
**Last Updated**: [YYYY-MM-DD]
```

---

## Decision Tree: Which Protocol to Use?

### Scenario 1: Multiple PRs Just Merged

```
PRs merged to master?
    ├─ YES → How many PRs?
    │       ├─ 1-3 PRs, simple changes
    │       │   └─ Direct documentation update
    │       │       └─ Create change tracking: docs/changes/docs-prs-{description}.md
    │       │       └─ Update docs directly
    │       │       └─ Mark complete in TODO.md
    │       │
    │       └─ 4+ PRs OR complex changes
    │           └─ Use PR-DOCUMENTATION-UPDATE-PROTOCOL
    │               └─ Create change tracking: docs/changes/docs-prs-{batch}.md
    │               └─ Follow 7-phase process
    │               └─ Update TODO.md with progress
    │               └─ → Proceed to Architecture Maintenance
    │
    └─ NO → See other scenarios
```

### Scenario 2: Architecture Changed

```
Architecture changed due to code changes?
    ├─ YES → How significant?
    │       ├─ Minor (1-2 docs affected)
    │       │   └─ Direct update to architecture docs
    │       │       └─ Create change tracking: docs/changes/architecture-{component}.md
    │       │       └─ Update affected C4 docs
    │       │       └─ → Proceed to Source Verification
    │       │
    │       └─ Major (3+ docs affected OR breaking changes)
    │           └─ Use ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL
    │               └─ Create change tracking: docs/changes/architecture-{topic}.md
    │               └─ Follow 6-phase process
    │               └─ Update TODO.md with progress
    │               └─ → Proceed to Source Verification
    │
    └─ NO → See other scenarios
```

### Scenario 3: Documentation Quality Concerns

```
Documentation quality concerns?
    ├─ Documentation accuracy questioned?
    │   └─ Use SOURCE-CODE-VERIFICATION-PROTOCOL
    │       └─ Create change tracking: docs/changes/verify-{topic}.md
    │       └─ Follow 7-phase process
    │       └─ Fix errors found
    │       └─ Update TODO.md with findings
    │
    ├─ Documentation gaps suspected?
    │   └─ Use DOCUMENTATION-GAP-ANALYSIS-PROTOCOL
    │       └─ Create change tracking: docs/changes/gap-analysis-{quarter}.md
    │       └─ Follow 7-phase process
    │       └─ Create remediation plan
    │       └─ Update TODO.md with plan
    │       └─ → Execute remediation
    │       └─ → Proceed to Source Verification
    │
    └─ Documentation out of date?
        └─ Determine cause
            ├─ PRs merged → Use PR-DOCUMENTATION-UPDATE-PROTOCOL
            ├─ Architecture changed → Use ARCHITECTURE-MAINTENANCE
            └─ Unknown → Use DOCUMENTATION-GAP-ANALYSIS-PROTOCOL
```

### Scenario 4: On Documentation Branch

```
Working on documentation branch?
    ├─ Need to merge master?
    │   └─ Use MERGE-AND-DOC-UPDATE-PROTOCOL
    │       └─ Create change tracking: docs/changes/merge-master-{date}.md
    │       └─ Follow merge protocol
    │       └─ Identify code changes from master
    │       └─ → Use PR-DOCUMENTATION-UPDATE-PROTOCOL for those changes
    │       └─ Update TODO.md
    │
    └─ Ready to merge docs to master?
        └─ Quality checks first
            ├─ Run SOURCE-CODE-VERIFICATION-PROTOCOL
            ├─ Fix any errors found
            └─ Create PR to master
```

### Scenario 5: New Feature Development

```
New feature developed?
    ├─ Feature merged to master via PR
    │   └─ Use PR-DOCUMENTATION-UPDATE-PROTOCOL
    │       └─ Document the PR
    │       └─ Update architecture docs (ARCHITECTURE-MAINTENANCE)
    │       └─ Create user guide (USER-GUIDE-GENERATION-PROTOCOL)
    │       └─ Verify accuracy (SOURCE-CODE-VERIFICATION)
    │
    └─ Feature still in development
        └─ Draft documentation in change tracking
            └─ Create: docs/changes/feature-{feature-name}.md
            └─ Update as feature evolves
            └─ Finalize after PR merged
```

---

## Integrated Workflow Examples

### Example 1: Weekly PR Documentation Sprint

**Scenario**: 12 PRs merged this week, need to document them all

**Workflow**:

1. **Planning (Monday morning)**
   ```bash
   # Update TODO.md
   - [ ] Document PRs 13084-13095 (12 PRs)
     - Protocol: PR-DOCUMENTATION-UPDATE-PROTOCOL
     - Estimated: 16 hours
     - Due: Friday EOD

   # Create change tracking
   touch docs/changes/docs-prs-week-of-2026-01-06.md
   ```

2. **Phase 1-2: Identify and Categorize (Monday, 2h)**
   - Use PR-DOCUMENTATION-UPDATE-PROTOCOL Phase 1-2
   - Create PR inventory
   - Categorize by type (Features, Security, etc.)
   - Update change tracking doc with inventory

3. **Phase 3-4: Update Architecture & Status (Tuesday-Wednesday, 8h)**
   - Use PR-DOCUMENTATION-UPDATE-PROTOCOL Phase 3-4
   - Update architecture docs
   - Update status.md
   - Update change tracking doc with changes made

4. **Phase 5: Create Analysis (Wednesday, 2h)**
   - Create CHANGES-SINCE-{COMMIT}.md
   - Document in change tracking

5. **Phase 6: Verification (Thursday, 3h)**
   - Use SOURCE-CODE-VERIFICATION-PROTOCOL (quick pass)
   - Verify key changes against source
   - Fix any errors
   - Update change tracking

6. **Phase 7: Commit and Complete (Friday, 1h)**
   - Commit all documentation changes
   - Archive change tracking document
   - Update TODO.md as complete
   - Update CLAUDE.md recent session work

**Total Time**: 16 hours (as estimated)

### Example 2: Quarterly Gap Analysis

**Scenario**: Quarterly documentation quality review

**Workflow**:

1. **Planning (Week 1)**
   ```bash
   # Update TODO.md
   - [ ] Q1 2026 Gap Analysis
     - Protocol: DOCUMENTATION-GAP-ANALYSIS-PROTOCOL
     - Estimated: 40 hours (8h analysis + 32h remediation)
     - Scheduled: Weeks 1-3 of January

   # Create change tracking
   touch docs/changes/gap-analysis-q1-2026.md
   ```

2. **Phase 1-3: Define, Identify, Measure (Week 1, 8h)**
   - Define documentation scope
   - Identify all gaps (entities, APIs, configs, user guides)
   - Calculate coverage metrics
   - Update change tracking with findings

3. **Phase 4: Prioritize (Week 1, 2h)**
   - Prioritize gaps (CRITICAL → HIGH → MEDIUM → LOW)
   - Update change tracking with priorities

4. **Phase 5: Remediation Plan (Week 1, 2h)**
   - Create sprint-based plan
   - Break into manageable tasks
   - Add tasks to TODO.md
   - Update change tracking with plan

5. **Phase 6: Execute Remediation (Week 2-3, 32h)**
   - Sprint 1: CRITICAL gaps (8h)
   - Sprint 2: HIGH gaps (12h)
   - Sprint 3: MEDIUM gaps (12h)
   - For each sprint:
     - Update docs
     - Use ARCHITECTURE-MAINTENANCE where applicable
     - Use SOURCE-VERIFICATION after updates
     - Update change tracking with progress

6. **Phase 7: Validate (Week 3, 4h)**
   - Re-run gap analysis
   - Compare before/after metrics
   - Create completion report
   - Update change tracking

7. **Completion**
   - Archive change tracking document
   - Update TODO.md as complete
   - Update CLAUDE.md recent session work
   - Schedule next gap analysis (Q2 2026)

**Total Time**: 48 hours (over 3 weeks)

### Example 3: Architecture Change from Feature

**Scenario**: Added new OpenStreamActivity feature, need to document architecture changes

**Workflow**:

1. **Planning**
   ```bash
   # Update TODO.md
   - [ ] Document OpenStreamActivity Architecture
     - Protocol: ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL
     - Estimated: 6 hours
     - Due: 2026-01-10

   # Create change tracking
   touch docs/changes/architecture-data-stream-activity.md
   ```

2. **Phase 1: Identify Changes (30min)**
   - Review PR 12555 (OpenStreamActivity)
   - Identify affected architecture areas:
     - Data Model
     - APIs
     - Synchronization
   - Update change tracking

3. **Phase 2: Update C4 Docs (3h)**
   - Update 05-data-model.md (add OpenStreamActivity entity)
   - Update 04-apis.md (add StreamTrackingService API)
   - Update 07-data-synchronization.md (add sync behavior)
   - Document in change tracking

4. **Phase 3: Cross-Reference (30min)**
   - Update cross-references between docs
   - Ensure terminology consistent

5. **Phase 4: Validate (1h)**
   - Use SOURCE-CODE-VERIFICATION-PROTOCOL (focused)
   - Verify OpenStreamActivity entity matches code
   - Verify StreamTrackingService API matches code
   - Fix any mismatches

6. **Phase 5: ADR (30min)**
   - Create ADR 0015: Stream Activity Tracking
   - Document decision rationale

7. **Phase 6: Commit (30min)**
   - Commit architecture updates
   - Archive change tracking
   - Update TODO.md as complete
   - Update CLAUDE.md

**Total Time**: 6 hours

### Example 4: Documentation Branch Merge

**Scenario**: Working on `users/mattw/claude-docs`, need to merge master

**Workflow**:

1. **Planning**
   ```bash
   # Update TODO.md
   - [ ] Merge master into docs branch
     - Protocol: MERGE-AND-DOC-UPDATE-PROTOCOL
     - Estimated: 4 hours
     - Due: 2026-01-08

   # Create change tracking
   touch docs/changes/merge-master-2026-01-08.md
   ```

2. **Phase 1-2: Pre-Merge and Execute (30min)**
   - Run MERGE-AND-DOC-UPDATE-PROTOCOL
   - Merge master into current branch
   - Resolve any conflicts

3. **Phase 3: Analyze Code Changes (1h)**
   - Review what code changed in master
   - Create code-changes-to-document.md
   - Create documentation-todo.md
   - Update change tracking

4. **Phase 4: Update Documentation (2h)**
   - Use PR-DOCUMENTATION-UPDATE-PROTOCOL for code changes
   - Update affected architecture docs
   - Update change tracking

5. **Phase 5-6: QA and Commit (30min)**
   - Validate build
   - Check documentation links
   - Commit changes
   - Update change tracking

7. **Completion**
   - Archive change tracking
   - Update TODO.md
   - Update CLAUDE.md

**Total Time**: 4 hours

---

## Quality Gates

### Gate 1: Before Committing Documentation

**Required**:
- [ ] Documentation updated for all identified changes
- [ ] Source code verification completed (at least spot-check)
- [ ] Code examples tested (if applicable)
- [ ] Cross-references validated
- [ ] TODO.md updated
- [ ] Change tracking document updated

**Optional but Recommended**:
- [ ] Peer review
- [ ] Stakeholder review
- [ ] Build validation

### Gate 2: Before Merging to Master

**Required**:
- [ ] All documentation changes committed
- [ ] Source code verification completed (comprehensive)
- [ ] No broken links
- [ ] No TODO/FIXME markers in docs
- [ ] CLAUDE.md updated with session work
- [ ] Change tracking documents archived

**Optional but Recommended**:
- [ ] Gap analysis run (if major changes)
- [ ] User guides updated (if user-facing changes)

### Gate 3: Periodic Quality Reviews

**Quarterly**:
- [ ] Gap analysis completed
- [ ] Coverage metrics calculated
- [ ] Remediation plan created (if needed)

**Semi-Annual**:
- [ ] Meta-review completed
- [ ] All architecture docs verified
- [ ] All code examples tested

---

## Best Practices

### 1. Always Create Change Tracking Documents

**Do**:
```bash
# For every documentation effort, create tracking doc
touch docs/changes/docs-prs-batch-5.md
# Update throughout the work
# Archive when complete
```

**Don't**:
```bash
# Don't track work only in TODO.md
# Don't skip change tracking for "small" changes
# Don't delete change tracking docs (archive them)
```

### 2. Update TODO.md Frequently

**Do**:
- Update status after each phase/day
- Mark blockers immediately
- Update effort estimates if they change
- Archive completed items (don't delete)

**Don't**:
- Let TODO.md go stale (update at least daily)
- Mix completed and active work
- Skip updating when blocked

### 3. Follow Protocol Sequences

**Typical Sequence**:
```
PR-DOCUMENTATION-UPDATE
    ↓
ARCHITECTURE-DOCUMENTATION-MAINTENANCE
    ↓
SOURCE-CODE-VERIFICATION
    ↓
Update TODO.md
    ↓
Archive change tracking
    ↓
Update CLAUDE.md
```

### 4. Use Appropriate Protocols for Task Size

**Small Task** (< 2h):
- Direct documentation update
- Create change tracking doc
- Update TODO.md
- Skip formal protocol (but document what you did)

**Medium Task** (2-8h):
- Use appropriate protocol (simplified)
- Create change tracking doc
- Update TODO.md
- Follow key protocol phases

**Large Task** (> 8h):
- Use full protocol
- Create comprehensive change tracking
- Update TODO.md with phases
- Follow all protocol phases
- Consider breaking into multiple change tracking docs

### 5. Integrate Verification Early

**Do**:
```
Update documentation
    ↓
Verify against source IMMEDIATELY
    ↓
Fix errors found
    ↓
Commit
```

**Don't**:
```
Update all documentation
    ↓
Commit
    ↓
Verify later (errors now in git)
    ↓
Fix and re-commit
```

### 6. Archive, Don't Delete

**Change Tracking Archive Structure**:
```
docs/changes/
├── archive/
│   ├── 2025/
│   │   ├── Q4/
│   │   │   ├── docs-prs-december-batch-1.md
│   │   │   ├── gap-analysis-q4-2025.md
│   │   │   └── merge-master-2025-12-15.md
│   │   └── Q3/
│   │       └── [archived changes]
│   └── 2024/
│       └── [archived changes]
└── [active change tracking docs]
```

**Archive Process**:
```bash
# When change tracking complete
mkdir -p docs/changes/archive/2026/Q1/
mv docs/changes/docs-prs-batch-5.md docs/changes/archive/2026/Q1/

# Update TODO.md to mark complete
# Update CLAUDE.md with session work
```

---

## Monitoring and Continuous Improvement

### Weekly Metrics

Track in `docs/changes/metrics-weekly.md`:

```markdown
# Week of 2026-01-06

## Work Completed
- PRs documented: 12
- Architecture docs updated: 5
- Change tracking docs created: 2
- Change tracking docs archived: 1

## Time Tracking
- Estimated total: 20h
- Actual total: 18h
- Variance: -2h (under estimate)

## Protocol Usage
- PR-DOCUMENTATION-UPDATE-PROTOCOL: 1 use (16h)
- SOURCE-CODE-VERIFICATION-PROTOCOL: 1 use (2h)
- ARCHITECTURE-DOCUMENTATION-MAINTENANCE: Integrated with PR protocol

## Issues
- None this week

## Quality Metrics
- Documentation coverage: 87% (+2% from last week)
- Verification pass rate: 95%
- Errors found and fixed: 3
```

### Monthly Review

Track in `docs/changes/metrics-monthly.md`:

```markdown
# January 2026

## Summary
- Total documentation hours: 72h
- PRs documented: 45
- Architecture docs updated: 12
- Gap analysis: Not scheduled
- Coverage improvement: +5% (82% → 87%)

## Protocol Effectiveness
- PR-DOCUMENTATION-UPDATE: Saved ~4h per batch with structured approach
- SOURCE-VERIFICATION: Found 12 errors before commit (prevented bad docs)
- ARCHITECTURE-MAINTENANCE: Kept C4 docs current

## Improvements Needed
- Speed up architecture updates (taking longer than estimated)
- Add more code example testing
- Schedule next gap analysis (Feb 2026)
```

---

## Emergency Procedures

### Emergency 1: Documentation Severely Out of Date

**Symptoms**: New team member can't onboard, docs don't match code, customer complaint

**Response**:
1. **Immediate** (Day 1):
   - Run DOCUMENTATION-GAP-ANALYSIS (quick assessment)
   - Identify CRITICAL gaps only
   - Create emergency change tracking: `docs/changes/emergency-gap-{date}.md`
   - Update TODO.md with CRITICAL items

2. **Short-term** (Week 1):
   - Fix CRITICAL gaps only
   - Use SOURCE-CODE-VERIFICATION for fixes
   - Get stakeholder approval

3. **Medium-term** (Weeks 2-4):
   - Complete full DOCUMENTATION-GAP-ANALYSIS
   - Execute remediation plan
   - Implement preventive measures

### Emergency 2: Major Architectural Change Not Documented

**Symptoms**: Breaking change deployed, documentation still shows old architecture

**Response**:
1. **Immediate** (Hours):
   - Create emergency change tracking: `docs/changes/emergency-architecture-{component}.md`
   - Document what changed (even if rough)
   - Publish emergency update

2. **Short-term** (Days):
   - Use ARCHITECTURE-DOCUMENTATION-MAINTENANCE (full protocol)
   - Verify with SOURCE-CODE-VERIFICATION
   - Create ADR for the change

3. **Follow-up** (Week):
   - Add breaking change to release notes
   - Update user guides
   - Notify stakeholders

---

## Success Metrics

### Documentation Lifecycle Health

**Healthy**:
- ✅ TODO.md updated daily
- ✅ Change tracking docs created for all work
- ✅ Documentation coverage > 85%
- ✅ Protocols followed consistently
- ✅ CLAUDE.md updated after each session
- ✅ No documentation older than 3 months without verification

**Needs Attention**:
- ⚠️ TODO.md not updated in > 3 days
- ⚠️ Change tracking docs missing
- ⚠️ Documentation coverage 70-85%
- ⚠️ Protocols skipped occasionally
- ⚠️ Documentation 3-6 months old

**Critical**:
- 🔴 TODO.md not updated in > 1 week
- 🔴 No change tracking
- 🔴 Documentation coverage < 70%
- 🔴 Protocols not followed
- 🔴 Documentation > 6 months old

---

## Appendix A: Quick Reference

### Common Commands

```bash
# Start new documentation work
touch docs/changes/{topic}-{description}.md
# Update TODO.md with new task

# After completing work
mv docs/changes/{doc}.md docs/changes/archive/$(date +%Y)/Q{quarter}/
# Update TODO.md as complete
# Update CLAUDE.md recent session work

# Check documentation health
grep -r "TODO\|FIXME" docs/
find docs/design/ -mtime +90  # Find docs older than 90 days
```

### Protocol Selection Quick Guide

| Situation | Protocol | Time | Change Tracking |
|-----------|----------|------|-----------------|
| 4+ PRs merged | PR-DOCUMENTATION-UPDATE | 4-8h | `docs-prs-{batch}` |
| 1-3 PRs merged | Direct update | 1-3h | `docs-prs-{description}` |
| Architecture changed | ARCHITECTURE-MAINTENANCE | 4-20h | `architecture-{topic}` |
| Accuracy questioned | SOURCE-CODE-VERIFICATION | 2-8h | `verify-{topic}` |
| Quality review | DOCUMENTATION-GAP-ANALYSIS | 4-200h | `gap-analysis-{quarter}` |
| Merge master | MERGE-AND-DOC-UPDATE | 2-8h | `merge-master-{date}` |
| New user guide | USER-GUIDE-GENERATION | 8-40h | `user-guide-{feature}` |

---

## Appendix B: File Structure

```
CadLink/
├── .claude/
│   └── protocols/
│       ├── DOCUMENTATION-WORKFLOW-LIFECYCLE.md (this file)
│       ├── PR-DOCUMENTATION-UPDATE-PROTOCOL.md
│       ├── DOCUMENTATION-GAP-ANALYSIS-PROTOCOL.md
│       ├── ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL.md
│       ├── SOURCE-CODE-VERIFICATION-PROTOCOL.md
│       └── [other protocols...]
│
├── TODO.md (primary work tracking)
│
├── docs/
│   ├── changes/  (active change tracking)
│   │   ├── docs-prs-current-batch.md
│   │   ├── architecture-security-update.md
│   │   ├── verify-api-docs.md
│   │   └── archive/
│   │       ├── 2026/
│   │       │   └── Q1/
│   │       │       ├── docs-prs-batch-1.md
│   │       │       └── gap-analysis-q1.md
│   │       └── 2025/
│   │           └── Q4/
│   │               └── [archived changes...]
│   │
│   ├── design/ (architecture docs)
│   ├── reference/ (status, overview)
│   └── [other doc categories...]
│
└── CLAUDE.md (project overview, recent session work)
```

---

**For AI Assistants**: This is the master lifecycle protocol. Use it to:
1. Determine which protocol to use for a given task
2. Understand how protocols integrate
3. Manage work tracking (TODO.md and change tracking)
4. Follow best practices for documentation workflow

Always create change tracking documents and update TODO.md throughout the workflow.

**Typical Usage**: Reference this document at the start of any documentation work to determine the right workflow path.
