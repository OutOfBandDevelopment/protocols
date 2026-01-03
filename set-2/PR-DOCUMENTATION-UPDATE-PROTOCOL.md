# PR Documentation Update Protocol

**Purpose**: Standardized protocol for documenting merged Pull Requests (PRs) and updating project documentation to reflect code changes.

**Version**: 1.0
**Last Updated**: 2026-01-03
**Target**: Documentation branches tracking merged PRs

---

## Overview

This protocol provides a systematic approach to:
1. **Review merged PRs** since a specific commit/date
2. **Categorize PRs** by type and impact
3. **Update architecture documentation** to reflect code changes
4. **Update status.md** with PR summaries
5. **Create analysis documents** for significant changes
6. **Validate completeness** of documentation updates

**Key Concept**: When PRs are merged to master, documentation must be updated to reflect what changed, why it changed, and how it affects the system.

---

## When to Use This Protocol

Use this protocol when:
- ✅ Multiple PRs have been merged since last documentation update
- ✅ You need to catch up documentation with master branch
- ✅ Preparing for a release and need complete PR documentation
- ✅ Conducting periodic documentation maintenance (weekly/monthly)
- ✅ New team member needs to understand recent changes

Skip this protocol when:
- ❌ Only 1-2 trivial PRs merged (document directly)
- ❌ PRs only contain documentation changes (no code changes)
- ❌ Working on feature branch (not master)

---

## Prerequisites

Before starting, verify:
- [ ] You have access to the git repository
- [ ] You have access to the PR system (Azure DevOps, GitHub, etc.)
- [ ] You know the starting commit/date for PR review
- [ ] Architecture documentation exists in `docs/design/`
- [ ] Status tracking exists in `docs/reference/status.md`
- [ ] You have permission to update documentation

---

## Phase 1: Identify Merged PRs

### 1.1 Determine Review Range

**Purpose**: Identify which PRs to document

**Methods**:

**Option A: Since Last Documented Commit (RECOMMENDED)**
```bash
# Get current master/main commit hash for tracking
CURRENT_MASTER=$(git rev-parse master)  # or 'main' if using main branch
echo "Current master: ${CURRENT_MASTER}"

# Find the last documented commit
# Check metadata in:
# - CLAUDE.md (Recent Session Work - look for "documented to commit XXX")
# - docs/reference/status.md (look for last "Documentation Range" commit)
# - docs/changes/archive/ (check most recent archived change tracking doc)

LAST_COMMIT="ff6ca2ce"  # Example: Last documented commit hash

# List commits to document
git log --oneline ${LAST_COMMIT}..HEAD

# Count commits
git log --oneline ${LAST_COMMIT}..HEAD | wc -l
```

**Option B: Since Specific Date**
```bash
# List PRs/commits since date
git log --oneline --since="2025-11-01" --until="2025-12-17"

# With merge commits only
git log --oneline --merges --since="2025-11-01"
```

**Option C: Since Tag/Release**
```bash
# List PRs since last tag
git log --oneline $(git describe --tags --abbrev=0)..HEAD
```

### 1.2 Extract PR Information

**Purpose**: Get PR numbers, titles, and metadata

**For Azure DevOps**:
```bash
# Extract PR numbers from merge commits
git log ${LAST_COMMIT}..HEAD --merges --oneline | grep -oP "Merged PR \K\d+" > .pr-review/pr-numbers.txt

# Create PR list with details
cat .pr-review/pr-numbers.txt | while read PR_NUM; do
    echo "PR $PR_NUM"
    # Use Azure DevOps CLI or API to get PR details
    # az repos pr show --id $PR_NUM --query "{title:title, author:createdBy.displayName, date:creationDate}"
done > .pr-review/pr-details.txt
```

**For GitHub**:
```bash
# Extract PR numbers from merge commits
git log ${LAST_COMMIT}..HEAD --merges --oneline | grep -oP "#\K\d+" > .pr-review/pr-numbers.txt

# Use GitHub CLI to get PR details
cat .pr-review/pr-numbers.txt | while read PR_NUM; do
    gh pr view $PR_NUM --json number,title,author,mergedAt
done > .pr-review/pr-details.json
```

**Manual Method**:
```bash
# Create working directory
mkdir -p .pr-review

# List merge commits with full messages
git log ${LAST_COMMIT}..HEAD --merges --format="%H|%s|%an|%ad" > .pr-review/merge-commits.txt

# Review and extract PR numbers manually
code .pr-review/merge-commits.txt
```

### 1.3 Create PR Inventory

**Purpose**: Create structured list of all PRs to document

Create `.pr-review/pr-inventory.md`:

```markdown
# PR Inventory - [Date Range]

**Review Range**: [commit-hash] to HEAD
**Date Range**: [start-date] to [end-date]
**Total PRs**: [count]

## PRs to Document

| PR # | Title | Author | Merged Date | Files Changed | Type |
|------|-------|--------|-------------|---------------|------|
| 13083 | Migrate Raygun API key | [author] | 2025-12-10 | 3 | Configuration |
| 13074 | Clean up SyncMetadata locks | [author] | 2025-12-09 | 2 | Performance |
| 13040 | Password hashing enhancements | [author] | 2025-12-08 | 4 | Security |
| ... | ... | ... | ... | ... | ... |

## Summary Statistics

**By Type**:
- Major Features: [count]
- Security: [count]
- Performance: [count]
- Bug Fixes: [count]
- Infrastructure: [count]
- Tests: [count]

**By Priority** (estimated):
- CRITICAL: [count] - Breaking changes, security fixes
- HIGH: [count] - New features, significant changes
- MEDIUM: [count] - Improvements, minor features
- LOW: [count] - Internal refactors, test updates
```

---

## Phase 2: Categorize PRs

### 2.1 Apply PR Categorization Framework

**Purpose**: Group PRs by type for targeted documentation updates

**Standard Categories**:

| Category | Description | Documentation Impact | Priority |
|----------|-------------|----------------------|----------|
| **Major Features** | New functionality, significant enhancements | HIGH - User guides, architecture, API docs | CRITICAL |
| **Security** | Security fixes, compliance updates | CRITICAL - Security docs, architecture | CRITICAL |
| **Performance** | Performance improvements, optimizations | MEDIUM - Architecture, best practices | HIGH |
| **Bug Fixes** | Defect corrections | LOW-MEDIUM - Release notes, affected docs | MEDIUM |
| **Infrastructure** | Build, deployment, configuration | MEDIUM - Setup docs, deployment docs | HIGH |
| **Database** | Schema changes, migrations | HIGH - Data model, migration docs | HIGH |
| **Tests** | Test additions, test infrastructure | LOW - Testing docs only | LOW |
| **Refactoring** | Code cleanup, no behavior change | LOW - Minimal doc impact | LOW |
| **Documentation** | Doc-only changes | NONE - Already documented | NONE |

### 2.2 Categorize Each PR

**Process**:

For each PR in inventory:
1. Review PR title and description
2. Review files changed (`.cs`, `.tsx`, `.sql`, `.md`, etc.)
3. Determine primary category
4. Assess documentation impact (CRITICAL/HIGH/MEDIUM/LOW)
5. Assign to category in inventory

**Decision Tree**:
```
PR Review
    ├─> Changes to authentication/authorization? → Security
    ├─> New user-facing feature? → Major Features
    ├─> Database schema change? → Database
    ├─> Performance optimization? → Performance
    ├─> Configuration/deployment change? → Infrastructure
    ├─> Bug fix? → Bug Fixes
    ├─> Test-only changes? → Tests
    ├─> Code cleanup, no behavior change? → Refactoring
    └─> Documentation only? → Documentation
```

### 2.3 Create Categorized PR List

Update `.pr-review/pr-inventory.md`:

```markdown
## PRs by Category

### Major Features (CRITICAL Priority)

**PR 12555 - Add OpenStreamActivity table**
- **Author**: [name]
- **Merged**: 2025-11-15
- **Files**: 8 files (5 .cs, 2 .sql, 1 .md)
- **Summary**: Track stream open/close activity
- **Doc Impact**: HIGH - Data model, sync docs, user guide
- **Docs to Update**:
  - [ ] `docs/design/05-data-model.md` - Add OpenStreamActivity table
  - [ ] `docs/design/07-data-synchronization.md` - Add sync behavior
  - [ ] `docs/user-guide/stream-tracking.md` - User guide (if needed)

**PR 12997 - ForeignFile subfoldering**
- **Author**: [name]
- **Merged**: 2025-12-01
- **Files**: 12 files (10 .cs, 1 .sql, 1 .md)
- **Summary**: Organize foreign files in subfolders
- **Doc Impact**: HIGH - Data model, architecture
- **Docs to Update**:
  - [ ] `docs/design/05-data-model.md` - Update ForeignFile structure
  - [ ] `docs/design/09-deployment-architecture.md` - File organization

### Security (CRITICAL Priority)

**PR 13040 - Password hashing enhancements**
- **Author**: [name]
- **Merged**: 2025-12-08
- **Files**: 4 files (4 .cs)
- **Summary**: Improve password security with better hashing
- **Doc Impact**: CRITICAL - Security docs, best practices
- **Docs to Update**:
  - [ ] `docs/design/06-authentication-security.md` - Password hashing
  - [ ] `docs/design/best-practices.md` - Security guidelines

[Continue for all categories...]
```

---

## Phase 3: Update Architecture Documentation

### 3.1 Map PRs to Architecture Documents

**Purpose**: Identify which C4 model documents need updates

**Standard Architecture Documents** (based on C4 model):

| Document | Content | Affected By |
|----------|---------|-------------|
| `01-system-context.md` | System boundaries, external systems | Infrastructure, Major Features |
| `02-containers.md` | High-level containers (apps, services) | Major Features, Infrastructure |
| `03-components.md` | Component architecture | Major Features, Refactoring |
| `04-apis.md` | API definitions, contracts | Major Features, Bug Fixes |
| `05-data-model.md` | Database schema, entities | Database, Major Features |
| `06-authentication-security.md` | Security architecture | Security, Major Features |
| `07-data-synchronization.md` | Sync mechanisms | Major Features, Performance |
| `08-configuration-management.md` | Configuration, settings | Infrastructure, Major Features |
| `09-deployment-architecture.md` | Deployment, infrastructure | Infrastructure |
| `10-testing-strategy.md` | Testing approach | Tests |

**Mapping Process**:

For each categorized PR:
1. Review PR files changed
2. Determine affected architecture areas
3. Map to architecture documents
4. Note specific sections to update

**Example Mapping**:

```markdown
## Architecture Document Updates Required

### docs/design/05-data-model.md

**PRs Affecting This Document**: 3
- PR 12555 (OpenStreamActivity table)
- PR 12997 (ForeignFile subfoldering)
- PR 13010 (Patient index optimization)

**Updates Required**:
1. Add OpenStreamActivity entity definition
2. Update ForeignFile entity with subfolder structure
3. Add note about Patient table indexes

### docs/design/06-authentication-security.md

**PRs Affecting This Document**: 2
- PR 13040 (Password hashing enhancements)
- PR 13032 (ProtectedData utilities)

**Updates Required**:
1. Update password hashing section with new algorithm
2. Add ProtectedData utilities section
3. Update security best practices

[Continue for all architecture documents...]
```

### 3.2 Update Each Architecture Document

**Process** for each document:

1. **Read Current Document**
   ```bash
   code docs/design/05-data-model.md
   ```

2. **Review Relevant PRs**
   - Read PR descriptions
   - Review code changes in PR
   - Understand impact on architecture

3. **Update Document**
   - Add new sections for new entities/components
   - Update existing sections with changes
   - Add code examples if applicable
   - Update diagrams if needed

4. **Validate Accuracy**
   - Cross-reference with actual source code
   - Ensure terminology is consistent
   - Verify all PRs are documented

5. **Commit Changes**
   ```bash
   git add docs/design/05-data-model.md
   git commit -m "docs: update data model for PRs 12555, 12997, 13010"
   ```

**Example Update** (for OpenStreamActivity):

```markdown
### OpenStreamActivity Table

**Purpose**: Track when users open and close patient streams for auditing and compliance.

**Added**: PR 12555 (2025-11-15)

**Schema**:

| Column | Type | Description |
|--------|------|-------------|
| Id | GUID | Primary key |
| PatientId | GUID | Foreign key to Patient |
| UserId | GUID | Foreign key to User |
| OpenedAt | DateTime | When stream was opened |
| ClosedAt | DateTime? | When stream was closed (null if still open) |
| StreamType | String | Type of stream (EEG, EMG, etc.) |

**Synchronization**: See [07-data-synchronization.md](07-data-synchronization.md#openstreamactivity)

**Source**: `/current/src/Source/Database/Tables/OpenStreamActivity.cs:15`
```

---

## Phase 4: Update Status Tracking

### 4.1 Update status.md

**Purpose**: Maintain comprehensive list of all documented PRs

**Location**: `docs/reference/status.md`

**Format**:

```markdown
## Recent Changes

### Latest Documentation Update - [Date]

**Documentation Range**: [commit] to HEAD ([X] PRs)
**Date Range**: [start-date] to [end-date]
**Categories**: Major Features (X), Security (X), Performance (X), Bug Fixes (X), Infrastructure (X), Database (X), Tests (X)

#### Major Features

**PR 12555 - Add OpenStreamActivity table**
- **Merged**: 2025-11-15
- **Impact**: Data model, synchronization
- **Documentation**: Updated `05-data-model.md`, `07-data-synchronization.md`

**PR 12997 - ForeignFile subfoldering**
- **Merged**: 2025-12-01
- **Impact**: File storage, data model
- **Documentation**: Updated `05-data-model.md`, `09-deployment-architecture.md`

#### Security

**PR 13040 - Password hashing enhancements**
- **Merged**: 2025-12-08
- **Impact**: Authentication security
- **Documentation**: Updated `06-authentication-security.md`, `best-practices.md`

[Continue for all PRs...]

### Statistics

**Total PRs Documented**: [count]
**Architecture Docs Updated**: [count]
**Documentation Coverage**: [percentage]%
```

### 4.2 Update CLAUDE.md Recent Session Work

**Purpose**: Record this documentation session in CLAUDE.md

**Location**: `CLAUDE.md` → "Recent Session Work" section

**Format**:

```markdown
## Recent Session Work

**[YYYY-MM-DD]**: Comprehensive documentation update for [X] merged PRs ([commit] to HEAD)
- Updated status.md with all [X] PRs organized by category ([categories])
- Updated architecture documentation:
  - [doc-1]: [changes]
  - [doc-2]: [changes]
  - [doc-3]: [changes]
- Created [analysis-doc]: [purpose]
- Key features documented: [list key features]

[Previous sessions below...]
```

---

## Phase 5: Create Analysis Documents

### 5.1 Create PR Analysis Summary

**Purpose**: Provide detailed analysis of all PRs reviewed

Create `docs/reference/CHANGES-SINCE-[COMMIT].md`:

```markdown
# Changes Since [Commit Hash]

**Analysis Date**: [YYYY-MM-DD]
**Commit Range**: [commit-hash] to HEAD
**Date Range**: [start-date] to [end-date]
**Total PRs Analyzed**: [count]

---

## Executive Summary

This document provides a comprehensive analysis of [X] Pull Requests merged between [dates]. The PRs span [categories] and have significant impact on [areas].

**Key Highlights**:
- 🆕 **Major Features**: [count] new features ([list key features])
- 🔒 **Security**: [count] security enhancements ([list key items])
- ⚡ **Performance**: [count] performance improvements
- 🐛 **Bug Fixes**: [count] defect corrections
- 🗄️ **Database**: [count] schema changes

**Architecture Impact**: [HIGH/MEDIUM/LOW]
- [X] architecture documents updated
- [Y] new sections added
- [Z] sections revised

---

## Detailed PR Analysis

### Major Features

#### PR 12555 - Add OpenStreamActivity table

**Author**: [name]
**Merged**: 2025-11-15
**Files Changed**: 8 files (5 .cs, 2 .sql, 1 .md)

**Summary**:
Implements tracking for stream open/close activities. Adds new OpenStreamActivity table to database with synchronization support.

**Technical Details**:
- **New Table**: `OpenStreamActivity` with columns Id, PatientId, UserId, OpenedAt, ClosedAt, StreamType
- **New Classes**: `OpenStreamActivity.cs`, `OpenStreamActivityRepository.cs`, `OpenStreamActivityService.cs`
- **Sync Support**: Added to SyncMetadata tracking, client/server synchronization

**Architecture Impact**:
- ✅ **Data Model**: New entity added
- ✅ **Synchronization**: New sync behavior
- ⚠️ **Database Migration**: Required on upgrade
- ✅ **API**: New service methods for stream tracking

**Documentation Updates**:
- Updated `docs/design/05-data-model.md` - Added OpenStreamActivity section
- Updated `docs/design/07-data-synchronization.md` - Added sync behavior
- Updated `docs/reference/status.md` - Listed PR

**Breaking Changes**: None
**Migration Required**: Yes - database upgrade needed
**HIPAA Impact**: Yes - audit trail for compliance

**Source Files**:
- `/current/src/Source/Database/Tables/OpenStreamActivity.cs:1`
- `/current/src/Source/Services/OpenStreamActivityService.cs:1`

[Continue for all PRs...]

---

## Architecture Impact Assessment

### High Impact Areas

#### Data Model (05-data-model.md)
**PRs Affecting**: 3 (12555, 12997, 13010)
**Impact Level**: HIGH
**Changes**:
- Added 1 new table (OpenStreamActivity)
- Modified 2 existing entities (ForeignFile, Patient)
- Added indexes for performance

**Validation**: ✅ All changes documented and verified against source code

#### Security (06-authentication-security.md)
**PRs Affecting**: 2 (13040, 13032)
**Impact Level**: CRITICAL
**Changes**:
- Enhanced password hashing algorithm
- Added ProtectedData utilities for App-V environments
- Updated security best practices

**Validation**: ✅ All changes documented and verified against source code

[Continue for all high-impact areas...]

### Documentation Coverage

**Metrics**:
- Total PRs: [X]
- PRs Documented: [Y]
- Documentation Coverage: [Y/X * 100]%
- Architecture Docs Updated: [Z]
- Lines Added to Docs: [approximately]

**Quality Indicators**:
- ✅ All critical PRs documented
- ✅ Source code verified for accuracy
- ✅ Code examples included where applicable
- ✅ Cross-references updated
- ✅ Breaking changes clearly marked

---

## Next Steps

### Immediate Actions
- [ ] Review this analysis for completeness
- [ ] Validate all architecture doc updates
- [ ] Ensure all PR references are correct
- [ ] Commit all documentation changes

### Follow-Up Work
- [ ] Update user guides for new features (if applicable)
- [ ] Create migration guides for breaking changes (if applicable)
- [ ] Update API documentation (if applicable)
- [ ] Notify team of significant changes

### Future Documentation Needs
- [ ] Consider creating user guide for OpenStreamActivity feature
- [ ] Update troubleshooting guide with new error scenarios
- [ ] Review and update deployment docs if needed

---

**Analysis Completed**: [YYYY-MM-DD]
**Next Review**: After next batch of PRs
**Owner**: [Name]
```

---

## Phase 6: Quality Assurance

### 6.1 Documentation Validation Checklist

**Purpose**: Ensure documentation is accurate and complete

**Validation Steps**:

- [ ] **Completeness**: All PRs from range are documented
- [ ] **Accuracy**: Documentation matches source code
- [ ] **Categorization**: All PRs correctly categorized
- [ ] **Architecture**: All architecture docs updated
- [ ] **Status**: status.md updated with all PRs
- [ ] **CLAUDE.md**: Recent session work updated
- [ ] **Cross-references**: Links between docs are valid
- [ ] **Code Examples**: Examples are accurate and tested
- [ ] **Terminology**: Consistent terminology across all docs
- [ ] **Formatting**: Markdown formatted correctly
- [ ] **Breaking Changes**: Clearly marked if present
- [ ] **Migration Notes**: Included if schema/API changed

**Validation Commands**:

```bash
# Check for broken internal links
grep -r "](docs/" docs/ | sed 's/.*](\(docs\/[^)]*\)).*/\1/' | while read link; do
    [ -f "$link" ] || echo "Broken link: $link"
done

# Check for TODO markers
grep -r "TODO\|FIXME\|XXX" docs/

# Check for placeholder text
grep -r "\[YOUR_.*\]\|\[FILL.*IN\]" docs/

# Verify all PRs from range are documented
git log ${LAST_COMMIT}..HEAD --merges --oneline | grep -oP "Merged PR \K\d+" | sort > /tmp/pr-merged.txt
grep -r "PR [0-9]" docs/reference/status.md | grep -oP "PR \K\d+" | sort > /tmp/pr-documented.txt
comm -23 /tmp/pr-merged.txt /tmp/pr-documented.txt  # Should be empty
```

### 6.2 Source Code Verification

**Purpose**: Ensure documentation matches actual source code

**For Each Major Change**:

1. **Locate Source Code**:
   ```bash
   # Find the class/file mentioned in documentation
   find . -name "OpenStreamActivity.cs"
   ```

2. **Review Source**:
   ```bash
   code ./path/to/OpenStreamActivity.cs
   ```

3. **Verify Documentation**:
   - Check that entity properties match documentation
   - Verify method signatures match API docs
   - Ensure behavior matches description

4. **Update If Mismatch**:
   - Correct documentation to match source code
   - Add note about verification

**Example Verification Note**:

```markdown
**Source Verification**: ✅ Verified against `/current/src/Source/Database/Tables/OpenStreamActivity.cs:15` on 2026-01-03
```

### 6.3 Peer Review (Optional)

**Purpose**: Get second opinion on documentation accuracy

**Process**:
1. Create PR for documentation changes
2. Request review from developer who worked on code PRs
3. Address feedback
4. Merge when approved

---

## Phase 7: Commit and Finalize

### 7.1 Commit Documentation Changes

**Purpose**: Commit all documentation updates with clear messages

**Commit Strategy**:

```bash
# Stage architecture docs
git add docs/design/

# Commit architecture updates
git commit -m "docs: update architecture for PRs [list PR numbers]

Updated architecture documentation for [X] merged PRs:

Major Features:
- PR 12555: OpenStreamActivity table
- PR 12997: ForeignFile subfoldering

Security:
- PR 13040: Password hashing enhancements
- PR 13032: ProtectedData utilities

Performance:
- PR 13074: SyncMetadata thread safety

Database:
- PR 12555: OpenStreamActivity table
- PR 12997: ForeignFile subfoldering

Infrastructure:
- PR 13083: Raygun API key migration
- PR 12972: Service restart timeout

Architecture docs updated:
- 05-data-model.md: OpenStreamActivity, ForeignFile
- 06-authentication-security.md: Password hashing, ProtectedData
- 07-data-synchronization.md: SyncMetadata, OpenStreamActivity
- 08-configuration-management.md: Raygun API, SystemMetrics
- 09-deployment-architecture.md: Service restart, App-V

Total PRs documented: [X]
Commit range: [hash] to HEAD"

# Stage status updates
git add docs/reference/status.md
git add CLAUDE.md

# Commit status updates
git commit -m "docs: update status tracking for [X] merged PRs

Updated status.md and CLAUDE.md with [X] PRs from [commit] to HEAD

Categories:
- Major Features: [X]
- Security: [X]
- Performance: [X]
- Bug Fixes: [X]
- Infrastructure: [X]
- Database: [X]
- Tests: [X]"

# Stage analysis document
git add docs/reference/CHANGES-SINCE-*.md

# Commit analysis
git commit -m "docs: add comprehensive PR analysis for [commit] to HEAD

Created CHANGES-SINCE-[COMMIT].md with detailed analysis:
- [X] PRs analyzed and categorized
- Architecture impact assessment
- Documentation coverage metrics
- Source code verification notes"

# Push all changes
git push origin $(git branch --show-current)
```

### 7.2 Clean Up Working Files

**Purpose**: Archive working files

```bash
# Move PR review files to archive
mkdir -p .pr-review/archive/$(date +%Y-%m-%d)
mv .pr-review/*.txt .pr-review/archive/$(date +%Y-%m-%d)/
mv .pr-review/*.md .pr-review/archive/$(date +%Y-%m-%d)/

# Keep inventory at root for reference
cp .pr-review/archive/$(date +%Y-%m-%d)/pr-inventory.md .pr-review/
```

---

## Success Metrics

This protocol is successful when:

- ✅ **All PRs Documented**: Every merged PR is documented
- ✅ **Architecture Updated**: All architecture docs reflect current state
- ✅ **Status Updated**: status.md and CLAUDE.md are current
- ✅ **Source Verified**: Documentation matches actual code
- ✅ **Quality Validated**: All validation checks pass
- ✅ **Analysis Created**: Comprehensive analysis document exists

---

## Integration with Other Protocols

### Merge Protocol Integration

**Scenario**: You're on a documentation branch and merge master which has new PRs

**Workflow**:
1. Use **MERGE-AND-DOC-UPDATE-PROTOCOL** to merge master
2. Merge protocol identifies code changes from master
3. Use **PR-DOCUMENTATION-UPDATE-PROTOCOL** (this protocol) to document those changes
4. Result: Branch is current with master and all PRs are documented

### Source Code Verification Integration

**Scenario**: Need to verify documentation accuracy

**Workflow**:
1. Use **PR-DOCUMENTATION-UPDATE-PROTOCOL** to document PRs
2. Use **SOURCE-CODE-VERIFICATION-PROTOCOL** to validate accuracy
3. Correct any mismatches
4. Result: Documentation is accurate and verified

---

## Appendix A: Quick Reference

```bash
# Phase 1: Identify PRs
LAST_COMMIT="ff6ca2ce"
mkdir -p .pr-review
git log --oneline ${LAST_COMMIT}..HEAD | tee .pr-review/commits.txt
git log ${LAST_COMMIT}..HEAD --merges --oneline | grep -oP "Merged PR \K\d+" | tee .pr-review/pr-numbers.txt

# Phase 2: Categorize
# [Manual review and categorization in pr-inventory.md]

# Phase 3: Update Architecture
code docs/design/05-data-model.md
# [Make updates based on PRs]
git add docs/design/

# Phase 4: Update Status
code docs/reference/status.md
code CLAUDE.md
git add docs/reference/status.md CLAUDE.md

# Phase 5: Create Analysis
code docs/reference/CHANGES-SINCE-${LAST_COMMIT}.md
git add docs/reference/

# Phase 6: Validate
grep -r "TODO" docs/
grep -r "](docs/" docs/ | sed 's/.*](\(docs\/[^)]*\)).*/\1/' | while read link; do [ -f "$link" ] || echo "Broken link: $link"; done

# Phase 7: Commit
git commit -m "docs: update for PRs [list]"
git push
```

---

## Appendix B: PR Categorization Examples

### Major Features
- New user-facing functionality
- New API endpoints
- New database tables
- New UI components
- Significant enhancements

### Security
- Authentication changes
- Authorization changes
- Encryption updates
- HIPAA compliance fixes
- Security vulnerability fixes

### Performance
- Query optimization
- Caching improvements
- Thread safety fixes
- Memory optimization
- Database indexing

### Bug Fixes
- Defect corrections
- Error handling improvements
- Edge case fixes
- UI bug fixes

### Infrastructure
- Build process changes
- Deployment changes
- Configuration changes
- CI/CD updates
- DevOps improvements

### Database
- Schema changes
- Migration scripts
- Index additions
- Constraint changes
- Stored procedure updates

### Tests
- New unit tests
- New integration tests
- Test infrastructure
- Test data updates
- Test fixes

---

**For AI Assistants**: This protocol should be executed when:
1. Multiple PRs have been merged since last documentation update
2. User requests "document recent PRs" or similar
3. Preparing for a release
4. Conducting periodic documentation maintenance

Use TodoWrite tool to track progress through all 7 phases.

**Typical Execution Time**: 2-8 hours depending on PR count and complexity.
