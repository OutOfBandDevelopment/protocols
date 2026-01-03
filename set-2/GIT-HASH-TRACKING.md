# Git Hash Tracking for Documentation

**Purpose**: Track git commit hashes to easily detect changes requiring documentation updates.

**Version**: 1.0
**Last Updated**: 2026-01-03

---

## Overview

Documentation should track **git commit hashes from master (or main)** to:
1. Identify exactly what changed since last documentation update
2. Make `git diff` commands easier and more accurate
3. Provide clear audit trail of what was documented when
4. Enable quick detection of documentation drift

**Key Concept**: Store the master (or main) branch commit hash in documentation metadata, then use `git diff {last-hash}..HEAD` to see what changed.

---

## Where to Track Git Hashes

### 1. Architecture Documentation Metadata

**Add to each architecture document** (e.g., `docs/design/05-data-model.md`):

```markdown
---

**Document Version**: 2.3
**Last Updated**: 2026-01-03
**Last Doc Update Against**: `a1b2c3d4` (master commit when doc last updated)
**Verified Against**: `a1b2c3d4` (source code commit verified)
**Changes Since Last Update**: 15 commits
**Next Review**: 2026-04-03 (quarterly)

**Git Commands**:
```bash
# See what changed since last doc update
git diff a1b2c3d4..HEAD -- '*.cs' '*.tsx' '*.sql'

# See commits since last update
git log --oneline a1b2c3d4..HEAD
```

---
```

### 2. Change Tracking Documents

**Add to change tracking documents** (e.g., `docs/changes/docs-prs-batch-5.md`):

```markdown
# Change Tracking: Document PRs Batch 5

**Created**: 2026-01-03
**Protocol**: PR-DOCUMENTATION-UPDATE-PROTOCOL
**Status**: In Progress
**Owner**: [Name]

**Git Tracking**:
- **Last Doc Update**: `ff6ca2ce` (2025-12-17 - last time docs were updated)
- **Current Master**: `a1b2c3d4` (2026-01-03 - master at start of this work)
- **Target Master**: `b2c3d4e5` (2026-01-10 - master at completion, if known)
- **Commits to Document**: `ff6ca2ce..a1b2c3d4` (20 commits)

**Git Commands**:
```bash
# See all changes since last doc update
git diff ff6ca2ce..a1b2c3d4

# See code changes only
git diff ff6ca2ce..a1b2c3d4 -- '*.cs' '*.tsx' '*.sql'

# See commits
git log --oneline ff6ca2ce..a1b2c3d4
```
```

### 3. CLAUDE.md Recent Session Work

**Format for session entries**:

```markdown
**2026-01-03**: Protocol reorganization and documentation workflow lifecycle
- Moved all protocols from `protocols/` to `.claude/protocols/`
- Created four new documentation protocols
- ...
- **Documented to commit**: `a1b2c3d4` (master 2026-01-03)
- **Commits covered**: `ff6ca2ce..a1b2c3d4` (20 commits)
```

### 4. Status Tracking (status.md)

**Add to status.md**:

```markdown
### Latest Documentation Update - 2026-01-03

**Documentation Range**: `ff6ca2ce..a1b2c3d4` (20 commits, 12 PRs)
**Master Commit**: `a1b2c3d4`
**Date Range**: 2025-12-17 to 2026-01-03
**Categories**: Major Features (3), Security (2), Performance (1), ...

**Git Commands**:
```bash
# Review what was documented
git diff ff6ca2ce..a1b2c3d4

# Review PRs merged
git log --oneline --merges ff6ca2ce..a1b2c3d4
```
```

---

## Workflow: Using Git Hash Tracking

### Starting New Documentation Work

```bash
# Get current master commit hash
git checkout master  # or 'main'
git pull
CURRENT_MASTER=$(git rev-parse master)
echo "Current master: ${CURRENT_MASTER}"

# Find last documented commit
# Check CLAUDE.md, status.md, or last archived change tracking doc
LAST_DOC_COMMIT="ff6ca2ce"  # Example from CLAUDE.md

# See what changed
echo "Commits to document:"
git log --oneline ${LAST_DOC_COMMIT}..${CURRENT_MASTER}

# Count commits
COMMIT_COUNT=$(git rev-list --count ${LAST_DOC_COMMIT}..${CURRENT_MASTER})
echo "Total commits to document: ${COMMIT_COUNT}"

# See file changes
echo "Files changed (code only):"
git diff --name-status ${LAST_DOC_COMMIT}..${CURRENT_MASTER} -- '*.cs' '*.tsx' '*.jsx' '*.sql' '*.csproj'

# Create change tracking document with this info
cat > docs/changes/docs-prs-batch-new.md <<EOF
# Change Tracking: Document Recent PRs

**Git Tracking**:
- Last Doc Update: ${LAST_DOC_COMMIT}
- Current Master: ${CURRENT_MASTER}
- Commits to Document: ${LAST_DOC_COMMIT}..${CURRENT_MASTER} (${COMMIT_COUNT} commits)
EOF
```

### During Documentation Work

```bash
# Check what files changed in specific areas
git diff ${LAST_DOC_COMMIT}..${CURRENT_MASTER} -- src/Database/Tables/

# See commit messages for context
git log ${LAST_DOC_COMMIT}..${CURRENT_MASTER} -- src/Database/Tables/

# See specific file changes
git show ${COMMIT_HASH}:path/to/file.cs
```

### Completing Documentation Work

```bash
# Record the final master commit documented
FINAL_MASTER=$(git rev-parse master)

# Update change tracking with completion info
cat >> docs/changes/docs-prs-batch-new.md <<EOF

## Completion

**Final Git State**:
- Documentation Updated Against: ${FINAL_MASTER}
- Commits Documented: ${LAST_DOC_COMMIT}..${FINAL_MASTER}
- Command to Review: git diff ${LAST_DOC_COMMIT}..${FINAL_MASTER}
EOF

# Update CLAUDE.md
# Update status.md
# Update architecture doc metadata
```

### Checking Documentation Freshness

```bash
# Get last documented commit from CLAUDE.md or architecture docs
LAST_DOC="a1b2c3d4"

# See what changed since then
git log --oneline ${LAST_DOC}..HEAD

# Count commits since last doc update
COMMITS_SINCE=$(git rev-list --count ${LAST_DOC}..HEAD)
echo "Commits since last doc update: ${COMMITS_SINCE}"

# See if architecture-affecting changes
git diff ${LAST_DOC}..HEAD -- '*.cs' '*.tsx' '*.sql' | wc -l

# If > 0 changes, documentation may need update
```

---

## Git Hash Storage Locations

### Primary Tracking (Must Have)

| Location | Purpose | Format | Update Frequency |
|----------|---------|--------|------------------|
| **CLAUDE.md** | Recent session work tracking | `Documented to commit: {hash}` | Every session |
| **status.md** | Latest documentation range | `Documentation Range: {hash}..{hash}` | Every documentation update |
| **Change tracking docs** | Detailed work tracking | `Last Doc Update: {hash}` | Per documentation effort |

### Secondary Tracking (Recommended)

| Location | Purpose | Format | Update Frequency |
|----------|---------|--------|------------------|
| **Architecture docs** | Doc-specific tracking | `Last Doc Update Against: {hash}` | When that doc is updated |
| **Analysis docs** | PR analysis metadata | `Commits Analyzed: {hash}..{hash}` | Per analysis |
| **Completion reports** | Gap analysis, etc. | `Verified Against: {hash}` | Per report |

---

## Best Practices

### ✅ Do

**Track master or main branch commits**:
```bash
# Always use master (or main), not your feature branch
git checkout master && git pull  # or 'git checkout main' if using main
MASTER_HASH=$(git rev-parse master)
```

**Use full hashes or short hashes consistently**:
```bash
# Full hash (40 chars)
git rev-parse master
# a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0

# Short hash (8 chars, consistent)
git rev-parse --short=8 master
# a1b2c3d4
```

**Store commit date with hash**:
```markdown
**Last Doc Update**: `a1b2c3d4` (2026-01-03)
```

**Provide git commands for reproducibility**:
```markdown
**Git Commands**:
```bash
git diff a1b2c3d4..HEAD
git log --oneline a1b2c3d4..HEAD
```
```

### ❌ Don't

**Don't track your feature branch commits**:
```bash
# DON'T
git rev-parse users/mattw/my-feature  # ❌ Wrong - feature branch

# DO
git rev-parse master  # ✅ Correct - master branch (or 'main' if using main)
```

**Don't use relative refs that change**:
```bash
# DON'T
git diff HEAD~10..HEAD  # ❌ Changes as you commit

# DO
git diff a1b2c3d4..HEAD  # ✅ Stable reference point
```

**Don't forget to update hashes**:
- Update CLAUDE.md after every session
- Update status.md after PR documentation
- Update architecture docs when updated

---

## Automation Helpers

### Get Last Documented Commit

```bash
# From CLAUDE.md
get_last_doc_commit_from_claude() {
    grep -m 1 "Documented to commit:" CLAUDE.md | grep -oP '`\K[a-f0-9]{8}'
}

# From status.md
get_last_doc_commit_from_status() {
    grep -m 1 "Documentation Range:" docs/reference/status.md | grep -oP '`\K[a-f0-9]{8}'
}

# From most recent archived change tracking
get_last_doc_commit_from_archive() {
    LATEST_ARCHIVE=$(find docs/changes/archive -name "*.md" -type f -printf '%T@ %p\n' | sort -rn | head -1 | cut -d' ' -f2)
    grep -m 1 "Last Doc Update:" "$LATEST_ARCHIVE" | grep -oP '`\K[a-f0-9]{8}'
}

# Use most recent
LAST_DOC=$(get_last_doc_commit_from_claude)
echo "Last documented commit: ${LAST_DOC}"
```

### Check Documentation Freshness

```bash
# Create script: check-doc-freshness.sh
#!/bin/bash

LAST_DOC=$(grep -m 1 "Documented to commit:" CLAUDE.md | grep -oP '`\K[a-f0-9]{8}')
CURRENT_MASTER=$(git rev-parse --short=8 master)

if [ "$LAST_DOC" = "$CURRENT_MASTER" ]; then
    echo "✅ Documentation is current with master"
    exit 0
fi

COMMITS_BEHIND=$(git rev-list --count ${LAST_DOC}..${CURRENT_MASTER})
echo "⚠️  Documentation is ${COMMITS_BEHIND} commits behind master"
echo ""
echo "Last documented: ${LAST_DOC}"
echo "Current master:  ${CURRENT_MASTER}"
echo ""
echo "Commits to document:"
git log --oneline ${LAST_DOC}..${CURRENT_MASTER}
echo ""
echo "Run: git diff ${LAST_DOC}..${CURRENT_MASTER}"
```

### Template: Start Documentation Work

```bash
#!/bin/bash
# setup-doc-work.sh {topic}

TOPIC=$1
LAST_DOC=$(grep -m 1 "Documented to commit:" CLAUDE.md | grep -oP '`\K[a-f0-9]{8}')
CURRENT_MASTER=$(git rev-parse --short=8 master)
COMMIT_COUNT=$(git rev-list --count ${LAST_DOC}..${CURRENT_MASTER})
DATE=$(date +%Y-%m-%d)

# Create change tracking document
cat > "docs/changes/${TOPIC}.md" <<EOF
# Change Tracking: ${TOPIC}

**Created**: ${DATE}
**Protocol**: [Protocol Name]
**Status**: In Progress
**Owner**: [Name]

**Git Tracking**:
- **Last Doc Update**: \`${LAST_DOC}\`
- **Current Master**: \`${CURRENT_MASTER}\`
- **Commits to Document**: \`${LAST_DOC}..${CURRENT_MASTER}\` (${COMMIT_COUNT} commits)

**Git Commands**:
\`\`\`bash
# See all changes
git diff ${LAST_DOC}..${CURRENT_MASTER}

# See code changes only
git diff ${LAST_DOC}..${CURRENT_MASTER} -- '*.cs' '*.tsx' '*.sql'

# See commits
git log --oneline ${LAST_DOC}..${CURRENT_MASTER}
\`\`\`

---

## Objective

[Fill in objective]

---

## Work Plan

[Fill in work plan]
EOF

echo "Created: docs/changes/${TOPIC}.md"
echo "Commits to document: ${COMMIT_COUNT}"
echo "Range: ${LAST_DOC}..${CURRENT_MASTER}"
```

---

## Examples

### Example 1: Architecture Document Metadata

```markdown
<!-- docs/design/05-data-model.md -->

# Data Model Architecture (C4 Level 3)

...content...

---

**Document Version**: 3.1
**Last Updated**: 2026-01-03
**Last Doc Update Against**: `a1b2c3d4` (master on 2026-01-03)
**Previous Update**: `ff6ca2ce` (master on 2025-12-17)
**Verified Against**: `a1b2c3d4` (source code verified)
**Changes Since Last**: 12 commits affecting data model
**Next Review**: 2026-04-03

**Git Commands**:
```bash
# See what changed since last update
git diff ff6ca2ce..a1b2c3d4 -- src/Database/Tables/

# See commits affecting data model
git log --oneline ff6ca2ce..a1b2c3d4 -- src/Database/Tables/ src/Entities/
```

**Change Log**:
- 2026-01-03 (`a1b2c3d4`): Added OpenStreamActivity entity, updated Patient relationships
- 2025-12-17 (`ff6ca2ce`): Updated ForeignFile with subfoldering
- 2025-12-01 (`d083ae76`): Major data model review and gap closure

---
```

### Example 2: Change Tracking Document

```markdown
<!-- docs/changes/docs-prs-december-batch-2.md -->

# Change Tracking: Document December PRs (Batch 2)

**Created**: 2026-01-03
**Protocol**: PR-DOCUMENTATION-UPDATE-PROTOCOL
**Status**: Complete
**Owner**: Claude AI
**Completed**: 2026-01-10

**Git Tracking**:
- **Last Doc Update**: `ff6ca2ce` (2025-12-17)
- **Current Master**: `a1b2c3d4` (2026-01-03 at start)
- **Final Master**: `b2c3d4e5` (2026-01-10 at completion)
- **Commits Documented**: `ff6ca2ce..b2c3d4e5` (20 commits, 12 PRs)

**Git Commands**:
```bash
# Review what was documented
git diff ff6ca2ce..b2c3d4e5

# Review PRs merged
git log --oneline --merges ff6ca2ce..b2c3d4e5

# Review specific changes
git log --oneline ff6ca2ce..b2c3d4e5 -- src/Database/Tables/
```

---

## Objective

Document 12 PRs merged between 2025-12-17 and 2026-01-03.

...
```

### Example 3: CLAUDE.md Session Entry

```markdown
**2026-01-10**: December PR documentation (batch 2)
- Documented 12 PRs merged between 2025-12-17 and 2026-01-03
- Updated architecture documentation (5 docs)
- Updated status.md with all PRs
- Created CHANGES-SINCE-FF6CA2CE.md analysis
- **Documented to commit**: `b2c3d4e5` (master on 2026-01-10)
- **Commits covered**: `ff6ca2ce..b2c3d4e5` (20 commits)
- **Git command**: `git diff ff6ca2ce..b2c3d4e5`
```

---

## Integration with Protocols

### PR-DOCUMENTATION-UPDATE-PROTOCOL

Phase 1.1: Add git hash capture:
```bash
LAST_DOC=$(grep -m 1 "Documented to commit:" CLAUDE.md | grep -oP '`\K[a-f0-9]{8}')
CURRENT_MASTER=$(git rev-parse --short=8 master)
echo "Documentation range: ${LAST_DOC}..${CURRENT_MASTER}"
```

### ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL

Phase 6: Add git hash to architecture doc metadata:
```markdown
**Last Doc Update Against**: `{current_master}` ({date})
```

### SOURCE-CODE-VERIFICATION-PROTOCOL

Phase 1: Specify git hash for verification:
```markdown
**Verified Against**: `{commit-hash}` (master on {date})
```

---

## Troubleshooting

### Issue: Don't know last documented commit

**Solution**:
```bash
# Check these locations in order:
# 1. CLAUDE.md (most reliable)
grep "Documented to commit:" CLAUDE.md | head -1

# 2. status.md
grep "Documentation Range:" docs/reference/status.md | head -1

# 3. Most recent archived change tracking
find docs/changes/archive -name "*.md" -type f -exec grep -l "Last Doc Update:" {} \; | xargs ls -t | head -1
```

### Issue: Documentation very out of date, many commits

**Solution**:
```bash
# Break into batches
LAST_DOC="ff6ca2ce"
CURRENT="$(git rev-parse --short=8 master)"

# Document in batches of ~20 commits
git log --oneline ${LAST_DOC}..${CURRENT} | tail -20 > batch1.txt
# Document batch 1
# Then continue with next batch
```

---

## Success Metrics

**Healthy**:
- ✅ CLAUDE.md has "Documented to commit" in most recent session
- ✅ status.md has "Documentation Range" with recent commit
- ✅ Architecture docs have "Last Doc Update Against" within 90 days
- ✅ Can run `git diff {last-doc}..HEAD` and get < 50 commits

**Needs Attention**:
- ⚠️ Last documented commit > 30 days old
- ⚠️ 50-100 commits since last doc update
- ⚠️ Architecture docs missing git hash metadata

**Critical**:
- 🔴 Last documented commit > 90 days old
- 🔴 > 100 commits since last doc update
- 🔴 Can't find last documented commit
- 🔴 No git hash tracking anywhere

---

**For AI Assistants**: Always:
1. Get current master commit hash at session start
2. Find last documented commit from CLAUDE.md/status.md
3. Calculate commits to document: `git diff {last}..{current}`
4. Include git tracking in all change tracking documents
5. Update CLAUDE.md with "Documented to commit" at session end

**For Developers**:
1. Check doc freshness: `git diff {last-doc}..HEAD`
2. Run provided automation scripts
3. Always reference master/main commits, not feature branches

---

**Last Updated**: 2026-01-03
**See Also**:
- [DOCUMENTATION-WORKFLOW-LIFECYCLE.md](DOCUMENTATION-WORKFLOW-LIFECYCLE.md)
- [PR-DOCUMENTATION-UPDATE-PROTOCOL.md](PR-DOCUMENTATION-UPDATE-PROTOCOL.md)
- [ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL.md](ARCHITECTURE-DOCUMENTATION-MAINTENANCE-PROTOCOL.md)
