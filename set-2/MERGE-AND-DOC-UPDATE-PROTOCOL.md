# Documentation Branch Master Merge Protocol

**Purpose**: Standardized protocol for merging master into a documentation branch and updating documentation to reflect code/solution changes from master.

**Version**: 2.0
**Last Updated**: 2025-11-21
**Target Branch**: Documentation branches only (e.g., `users/mattw/claude-docs`)

---

## Overview

This protocol provides a systematic approach to:
1. **Merge master** into a documentation-only branch
2. **Analyze code/solution changes** that came from master
3. **Update documentation** to reflect those code changes using the code review gap analysis methodology
4. **Validate completeness** of documentation updates

**Key Concept**: This is for documentation branches that track doc-only changes. When master is merged in, the protocol helps identify what code changes happened in master that now need documentation updates.

---

## Prerequisites

Before starting, verify:

- [ ] Current branch is a documentation branch (e.g., `users/mattw/claude-docs`, `docs/*`)
- [ ] Branch contains primarily documentation changes (not code changes)
- [ ] All local changes are committed or stashed
- [ ] Master branch is up-to-date locally
- [ ] No merge conflicts from previous attempts
- [ ] Documentation templates are available in `docs/prompts/`
- [ ] Code review infrastructure exists in `docs/code-review/`

---

## Phase 1: Pre-Merge Preparation

### 1.1 Verify Documentation Branch

**Purpose**: Confirm we're on a documentation branch, not a feature/code branch

**Steps**:

```bash
# Verify current branch
$CURRENT_BRANCH = git branch --show-current
echo "Current branch: $CURRENT_BRANCH"

# Check if it's a documentation branch
if ($CURRENT_BRANCH -notmatch "claude-docs|docs/") {
    Write-Warning "This branch may not be a documentation branch. Continue? (y/N)"
}
```

**Validation**:
- ✅ Branch name contains "docs" or "claude-docs"
- ✅ Recent commits are primarily documentation changes
- ⚠️ If branch has code changes, this protocol may not be appropriate

### 1.2 Understand What's New in Master

**Purpose**: Identify code/solution changes in master that need documentation

```bash
# Create protocol directory
mkdir -p .merge-protocol

# Capture current doc branch state
git log -1 --oneline > .merge-protocol/pre-merge-doc-branch.txt

# Find code changes in master that aren't in docs branch
# Focus on .cs, .tsx, .jsx, .sql files (code, not docs)
git log HEAD..master --oneline --name-only -- '*.cs' '*.tsx' '*.jsx' '*.sql' '*.csproj' > .merge-protocol/master-code-changes.txt

# Get summary of master commits since docs branch diverged
git log HEAD..master --oneline > .merge-protocol/master-new-commits.txt

# Count code files changed in master
git diff --name-only HEAD master -- '*.cs' '*.tsx' '*.jsx' '*.sql' > .merge-protocol/master-code-files-changed.txt
```

**Output Files**:
- `.merge-protocol/pre-merge-doc-branch.txt` - Current docs branch commit
- `.merge-protocol/master-code-changes.txt` - Code changes in master (with commit messages)
- `.merge-protocol/master-new-commits.txt` - All new master commits
- `.merge-protocol/master-code-files-changed.txt` - List of code files changed

**Key Question**: What code changes are in master that need to be documented?

### 1.3 Update Master Branch

**Purpose**: Ensure master is current before merge

```bash
# Fetch latest from remote
git fetch origin master

# Verify master is up-to-date
git log master..origin/master --oneline
```

**Validation**:
- If output is empty, master is current
- If commits appear, update master: `git checkout master && git pull && git checkout -`

---

## Phase 2: Execute Merge

### 2.1 Merge Master into Current Branch

**Purpose**: Integrate master changes into feature branch

```bash
# Merge master into current branch
git merge master -m "Merge master into $(git branch --show-current)"
```

**Expected Outcomes**:

**Scenario A: Clean Merge**
- ✅ Merge completes automatically
- ✅ No conflicts reported
- → Proceed to Phase 2.3

**Scenario B: Merge Conflicts**
- ⚠️ Git reports conflicts in specific files
- → Proceed to Phase 2.2

**Scenario C: Merge Aborted**
- ❌ Merge fails due to uncommitted changes
- → Commit or stash changes, retry

### 2.2 Resolve Merge Conflicts (If Applicable)

**Purpose**: Manually resolve conflicts before proceeding

**Steps**:

1. **List conflicted files**:
   ```bash
   git status | grep "both modified"
   ```

2. **For each conflicted file**:
   - Open file in editor
   - Locate conflict markers: `<<<<<<<`, `=======`, `>>>>>>>`
   - Determine correct resolution:
     - **Keep current branch**: Remove master changes, keep yours
     - **Keep master**: Remove your changes, keep master
     - **Merge both**: Combine changes logically
   - Remove conflict markers
   - Save file

3. **Stage resolved files**:
   ```bash
   git add <resolved-file>
   ```

4. **Complete merge**:
   ```bash
   git commit -m "Merge master into $(git branch --show-current) - resolved conflicts"
   ```

**Validation**:
```bash
# Verify no conflicts remain
git status | grep "Unmerged paths"  # Should return nothing

# Verify merge completed
git log -1 --merges  # Should show merge commit
```

### 2.3 Capture Post-Merge State

**Purpose**: Document what changed during merge

```bash
# Create merge protocol directory if needed
mkdir -p .merge-protocol

# Capture merge commit
git log -1 --oneline > .merge-protocol/post-merge-commit.txt

# Get full list of files changed from master
git diff --name-status master HEAD > .merge-protocol/post-merge-files.txt

# Get detailed change statistics
git diff --stat master HEAD > .merge-protocol/merge-stats.txt

# Get commit log of what was merged in
git log --oneline HEAD~1..master > .merge-protocol/merged-commits.txt
```

**Output Files**:
- `.merge-protocol/post-merge-commit.txt` - Merge commit hash
- `.merge-protocol/post-merge-files.txt` - All files changed from master
- `.merge-protocol/merge-stats.txt` - Change statistics (additions/deletions)
- `.merge-protocol/merged-commits.txt` - Commits brought in from master

---

## Phase 3: Analyze Code Changes from Master

### 3.1 Categorize Code Changes (Not Doc Changes)

**Purpose**: Understand what CODE/SOLUTION changes came from master that need documentation

**Process**:

1. **Read code commits from master**:
   ```bash
   cat .merge-protocol/master-code-changes.txt
   ```

2. **Categorize by code area (ignore .md files)**:

   Create `.merge-protocol/code-changes-to-document.md`:

   ```markdown
   # Code Changes from Master Requiring Documentation

   ## Backend Changes (.cs files)

   ### API Changes
   [List .cs files with public API changes]
   - File: [path/to/file.cs]
     - Commits: [hash1, hash2]
     - Summary: [what changed]
     - Public API Impact: YES/NO
     - Breaking Change: YES/NO

   ### Internal Changes
   [List .cs files with internal changes]
   - File: [path/to/file.cs]
     - Commits: [hash]
     - Summary: [what changed]
     - Needs Documentation: YES/NO/MAYBE

   ## Frontend Changes (.tsx, .jsx files)

   ### UI Component Changes
   [List UI files changed]
   - File: [path/to/component.tsx]
     - Commits: [hash]
     - Summary: [what changed]
     - User-Visible: YES/NO
     - Needs User Guide Update: YES/NO

   ### Internal UI Changes
   [List internal UI changes]

   ## Database Changes (.sql files)

   ### Schema Changes
   [List SQL files changed]
   - File: [path/to/migration.sql]
     - Commits: [hash]
     - Summary: [what changed]
     - Breaking: YES/NO
     - Needs Migration Docs: YES/NO

   ## Configuration Changes

   ### Build/Project Changes (.csproj, .sln)
   [List build file changes]
   - Needs Setup/Build Docs Update: YES/NO

   ### CI/CD Changes (.yml, .yaml)
   [List pipeline changes]
   - Needs DevOps Docs Update: YES/NO

   ## Summary

   **Total Code Files Changed**: [X]
   - Backend: [Y] files
   - Frontend: [Z] files
   - Database: [A] files
   - Configuration: [B] files

   **Documentation Impact**:
   - HIGH: [Count] changes requiring immediate doc updates
   - MEDIUM: [Count] changes needing doc consideration
   - LOW: [Count] internal changes, minimal doc impact
   ```

### 3.2 Extract Change Intent from Code Commits

**Purpose**: Understand why CODE changes were made (features, bugs, refactors, security fixes)

**Process**:

1. **Read commit messages for code changes only**:
   ```bash
   # Get commit messages for code files only
   git log HEAD..master --oneline -- '*.cs' '*.tsx' '*.jsx' '*.sql' '*.csproj'
   ```

2. **Group by intent**:

   Create `.merge-protocol/code-change-intent.md`:

   ```markdown
   # Code Change Intent Analysis (from Master)

   ## New Features Added
   - [Feature 1]:
     - Commits: [abc123, def456]
     - Files: [list code files]
     - **Docs Needed**: YES - User guide, API docs

   ## Bugs Fixed
   - [Bug 1]:
     - Commits: [jkl012]
     - Files: [list code files]
     - **Docs Needed**: MAYBE - If behavior changed

   ## API Changes/Refactoring
   - [Refactor 1]:
     - Commits: [pqr678]
     - Files: [list code files]
     - Breaking: YES/NO
     - **Docs Needed**: YES if public API changed

   ## Security/Compliance Fixes
   - [Security Fix 1]:
     - Commits: [stu901]
     - Files: [list code files]
     - **Docs Needed**: YES - Security docs, code review phase

   ## Performance Improvements
   - [Perf 1]:
     - Commits: [vwx234]
     - Files: [list code files]
     - User-Visible: YES/NO
     - **Docs Needed**: If user-visible performance changes

   ## Infrastructure/Build Changes
   - [Infra 1]:
     - Commits: [yza567]
     - Files: [list code files]
     - **Docs Needed**: YES if setup/build process changed
   ```

### 3.3 Map Code Changes to Documentation Needs

**Purpose**: Create explicit mapping of which docs need updates based on code changes

**Decision Tree**:

```
Code Change Type → Documentation Files to Update
───────────────────────────────────────────────────
Public API Changed (.cs)      → docs/api/[module].md
                              → docs/code-review/[phase].md

UI Component Changed (.tsx)   → docs/user-guide/[section].md
                              → docs/ui/[component].md

Database Schema Changed       → docs/database/schema.md
                              → docs/database/migrations.md

Security Fix Applied          → docs/security/[topic].md
                              → docs/code-review/[phase].md

Build Process Changed         → docs/setup/build.md
                              → docs/development/environment.md

New Feature Added             → docs/user-guide/[feature].md
                              → docs/api/[feature].md
                              → docs/code-review/[new-phase].md
```

**Output**:

Create `.merge-protocol/documentation-todo.md`:

```markdown
# Documentation Updates Required (Based on Master Code Changes)

## CRITICAL Priority (Do Immediately)

### 1. [Doc File Name]
**Trigger**: [Code change description]
**Code Files**: [list .cs/.tsx files that changed]
**Commits**: [hash1, hash2]
**Why**: [Breaking change / New public API / Security fix]
**Changes Needed**:
- [ ] Add section on [new feature]
- [ ] Update API reference for [method/class]
- [ ] Add code examples
**Estimated Effort**: [X] hours

### 2. [Doc File Name]
[Repeat]

## HIGH Priority (This Week)

### 1. [Doc File Name]
**Trigger**: [Code change description]
**Code Files**: [list files]
**Why**: [New feature / Significant refactor]
**Changes Needed**:
- [ ] Document new [feature]
- [ ] Update examples
**Estimated Effort**: [X] hours

## MEDIUM Priority (This Sprint)

### 1. [Doc File Name]
**Trigger**: [Code change description]
**Why**: [Bug fix with behavior change / Minor API update]
**Changes Needed**:
- [ ] Update [section]
**Estimated Effort**: [X] hours

## LOW Priority (Backlog)

### 1. [Doc File Name]
**Trigger**: [Code change description]
**Why**: [Internal refactor / No user-visible change]
**Changes Needed**:
- [ ] Consider adding note about [implementation detail]
**Estimated Effort**: [X] hours

---

## Summary

**Total Documentation Files to Update**: [X]
- CRITICAL: [Y] files
- HIGH: [Z] files
- MEDIUM: [A] files
- LOW: [B] files

**Total Estimated Effort**: [X] hours

**Recommended Approach**:
- Complete CRITICAL updates today
- Schedule HIGH updates this week
- Create Code Review Phase if total effort > 8 hours
```

---

## Phase 4: Update Documentation Using Code Review Framework

### 4.1 Apply Code Review Gap Analysis

**Purpose**: Use systematic code review methodology for documentation updates

**Reference**: `docs/prompts/code-review-gap-analysis-and-execution.md`

**Process**:

1. **Identify Documentation Gaps**:

   For each change category from Phase 3.1, determine if existing documentation:
   - ❌ Doesn't exist (NEW documentation needed)
   - ⚠️ Exists but is outdated (UPDATE needed)
   - ✅ Is accurate and current (NO CHANGE needed)

2. **Create Documentation Review Phases**:

   If significant changes detected, create new code review phases in `docs/code-review/TODO.md`:

   ```markdown
   ### Phase XX: [Description of Changes from Master] 🔴
   **Status**: Pending
   **Priority**: CRITICAL/HIGH/MEDIUM/LOW ([Category])
   **Estimated Effort**: [X] days
   **Trigger**: Master merge on [date] brought in [change summary]

   - [ ] **XX.1 Analyze Impact** - Review changes from commits [abc..xyz]
     - File: `XX-[descriptive-name].md`

   - [ ] **XX.2 Gap Analysis** - Compare current vs. desired documentation state
     - File: `XX-[descriptive-name].md`

   - [ ] **XX.3 Documentation Update** - Create/update affected documentation
     - File: `XX-[descriptive-name].md`

   - [ ] **XX.4 Validation** - Verify documentation accuracy
     - File: `XX-[descriptive-name].md`

   **Rationale**: Master merge introduced [specific changes]. Documentation must reflect [specific areas].
   ```

### 4.2 Execute High-Priority Documentation Updates

**Purpose**: Immediately update critical documentation

**Priority Rules** (from code review protocol):
1. **CRITICAL**: Security changes, HIPAA compliance changes, breaking API changes
2. **HIGH**: New features, significant refactors, architecture changes
3. **MEDIUM**: Bug fixes with behavioral changes, minor API updates
4. **LOW**: Internal refactors, performance improvements without API changes

**For Each High-Priority Update**:

1. **Create Phase Document** (if warranted):

   Use template from `code-review-gap-analysis-and-execution.md`:

   ```markdown
   # Phase XX: [Change Description from Master]

   **Status**: Complete
   **Audit Date**: [YYYY-MM-DD]
   **Priority**: [P0/P1/P2/P3]
   **Estimated Effort**: [X hours/days]
   **Trigger**: Master merge commit [hash]

   ---

   ## Executive Summary

   [What was merged, why documentation needed update, business impact]

   ### Changes from Master

   | Commit | Author | Date | Summary |
   |--------|--------|------|---------|
   | [hash] | [name] | [date] | [summary] |

   ### Documentation Impact

   | Document | Change Type | Priority | Status |
   |----------|-------------|----------|--------|
   | [file]   | [NEW/UPDATE/DELETE] | [CRITICAL/HIGH] | [✅/⚠️/❌] |

   ---

   ## 1. Current State Analysis

   ### 1.1 Code Changes from Master

   **Change 1: [Description]**

   ```csharp
   // File: /current/src/Source/[Path]/[File].cs:[Line]
   // Added in commit: [hash]
   [Code example]
   ```

   **Impact**: [How this affects system behavior/API/user experience]

   **Documentation Required**: [What needs to be documented]

   ### 1.2 Existing Documentation State

   **Current Documentation**: [What exists today in docs/]

   **Gaps Identified**:
   - ❌ Gap 1: [Specific missing information]
   - ❌ Gap 2: [Specific outdated information]

   ---

   ## 2. Documentation Updates Required

   ### 2.1 [Document Name 1]

   **File**: `docs/[path]/[file].md`
   **Change Type**: NEW / UPDATE / DELETE
   **Priority**: CRITICAL / HIGH / MEDIUM / LOW

   **Current Content**: [What exists or doesn't exist]
   **Required Content**: [What should be added/changed]
   **Rationale**: [Why this documentation is needed]

   **Update Plan**:
   - [ ] Step 1
   - [ ] Step 2
   - [ ] Step 3

   ### 2.2 [Document Name 2]

   [Repeat structure]

   ---

   ## 3. Remediation Roadmap

   ### 3.1 Immediate Updates (Today)

   **Priority**: CRITICAL
   **Effort**: [X] hours

   **Tasks**:
   - [ ] Update `docs/[file1].md` - Add section on [topic] (1h)
   - [ ] Update `docs/[file2].md` - Revise [section] to reflect [change] (2h)

   **Success Criteria**:
   - ✅ All CRITICAL documentation gaps closed
   - ✅ Breaking changes fully documented

   ### 3.2 Short-Term Updates (This Week)

   **Priority**: HIGH
   **Effort**: [Y] hours

   **Tasks**:
   - [ ] Create `docs/[new-file].md` - Document [new feature] (4h)
   - [ ] Update `docs/code-review/TODO.md` - Add tracking for [topic] (1h)

   **Success Criteria**:
   - ✅ All HIGH priority documentation complete
   - ✅ User-facing changes documented

   ### 3.3 Long-Term Updates (This Sprint)

   **Priority**: MEDIUM/LOW
   **Effort**: [Z] hours

   **Tasks**:
   - [ ] Refactor `docs/[file].md` for clarity (3h)
   - [ ] Add examples to `docs/[file].md` (2h)

   ---

   ## 4. Validation Checklist

   - [ ] All code changes from master identified
   - [ ] All affected documentation files listed
   - [ ] Priority assigned based on change impact
   - [ ] Update plan created for each document
   - [ ] CRITICAL updates completed
   - [ ] HIGH updates scheduled
   - [ ] Cross-references updated (links between docs)
   - [ ] Code examples tested and accurate
   - [ ] Terminology consistent across docs

   ---

   ## 5. Conclusion

   **Summary**: [1-2 sentence summary of merge and documentation impact]

   **Documentation Files Updated**: [Count]
   **Documentation Files Created**: [Count]
   **Estimated Effort**: [Total hours]

   **Next Steps**:
   1. Complete CRITICAL updates
   2. Schedule HIGH priority updates
   3. Monitor for additional changes in next master merge

   ---

   **Audit Completed**: [YYYY-MM-DD]
   **Next Review**: [After next master merge]
   **Owner**: [Responsible person]
   ```

2. **Execute Documentation Updates**:

   For each documentation file identified:

   ```bash
   # Open file for editing
   code docs/[path]/[file].md

   # Make updates according to plan
   # [Edit file content]

   # Validate changes
   # [Review diff]

   # Commit updates
   git add docs/[path]/[file].md
   git commit -m "docs: update [file] to reflect changes from master merge [commit-hash]"
   ```

3. **Update Code Review TODO**:

   Edit `docs/code-review/TODO.md`:

   ```markdown
   ### Statistics
   - **Total Follow-Up Tasks**: [Updated count]
   - **Completed**: [Updated count with new phase]
   - **Pending**: [Updated count]
   - **Progress**: [Updated percentage]

   ### Recent Phases

   #### Phase XX: Documentation Update from Master Merge ✅
   **Status**: Complete
   **Completed**: [YYYY-MM-DD]
   **Priority**: [CRITICAL/HIGH/MEDIUM/LOW]
   **Trigger**: Master merge commit [hash]

   **Key Changes**:
   - ✅ Updated `docs/[file1].md` to reflect [change]
   - ✅ Created `docs/[file2].md` for new [feature]
   - ✅ Revised `docs/[file3].md` with [update]

   **Files Changed**: [X] files updated, [Y] files created
   **Effort**: [Z] hours
   ```

---

## Phase 5: Quality Assurance

### 5.1 Documentation Validation

**Purpose**: Ensure documentation updates are complete and accurate

**Checklist**:

- [ ] **Accuracy**: All code examples compile and run
- [ ] **Completeness**: All changes from master documented
- [ ] **Consistency**: Terminology matches across all docs
- [ ] **Cross-references**: Links between docs are updated and valid
- [ ] **Examples**: New features have usage examples
- [ ] **Breaking Changes**: Called out prominently if applicable
- [ ] **Migration Guide**: Included if API/behavior changed
- [ ] **Formatting**: Markdown formatted correctly, renders properly

**Validation Commands**:

```bash
# Check for broken internal links
grep -r "](docs/" docs/ | sed 's/.*](\(docs\/[^)]*\)).*/\1/' | while read link; do
    [ -f "$link" ] || echo "Broken link: $link"
done

# Check for TODO markers left in docs
grep -r "TODO\|FIXME\|XXX" docs/

# Check for placeholder text
grep -r "\[YOUR_.*\]\|\[FILL.*IN\]" docs/
```

### 5.2 Build Verification

**Purpose**: Ensure merge didn't break the build

```bash
# Restore NuGet packages
dotnet restore

# Build solution
dotnet build

# Run tests (if applicable)
dotnet test

# Check for build warnings
dotnet build 2>&1 | grep -i "warning"
```

**Acceptance Criteria**:
- ✅ Solution builds successfully
- ✅ No new build warnings introduced
- ✅ All tests pass (or same failures as before merge)

### 5.3 Code Review TODO Reconciliation

**Purpose**: Ensure code review tracking is current

**Steps**:

1. **Verify Phase Numbering**:
   - Phases should be sequential (no gaps)
   - New phases should use next available number

2. **Verify Statistics**:
   ```markdown
   # Check math
   - Total tasks = Completed + In Progress + Pending ✅
   - Progress % = (Completed / Total) × 100 ✅
   - Priority counts match phase listings ✅
   ```

3. **Verify Next Actions**:
   - Next Actions section lists highest priority pending phases
   - Order: CRITICAL → HIGH → MEDIUM → LOW
   - Each has clear impact statement

---

## Phase 6: Commit and Document

### 6.1 Commit Documentation Updates

**Purpose**: Commit all documentation changes with clear messages

**Commit Strategy**:

```bash
# Stage documentation files
git add docs/

# Commit with descriptive message
git commit -m "docs: update documentation for master merge $(git log -1 --format=%h master)

- Updated docs/[file1].md: [change description]
- Created docs/[file2].md: [change description]
- Revised docs/code-review/TODO.md: Added Phase XX
- Updated docs/code-review/XX-*.md: [phase description]

Trigger: Master merge brought in [X] commits with [summary of changes]
Affected: [Y] documentation files
Effort: [Z] hours"

# If phase documents were created
git add docs/code-review/XX-*.md
git commit -m "docs: add code review Phase XX for master merge documentation

Phase XX: [Description]
- Priority: [CRITICAL/HIGH/MEDIUM/LOW]
- Effort: [X] hours
- Files affected: [list]
- Trigger: Master merge commit [hash]"
```

### 6.2 Update Merge Protocol Tracking

**Purpose**: Create audit trail for future reference

Create `.merge-protocol/MERGE-SUMMARY.md`:

```markdown
# Master Merge Summary - [YYYY-MM-DD]

## Merge Information

**Branch**: [current-branch-name]
**Master Commit**: [master-commit-hash]
**Merge Commit**: [merge-commit-hash]
**Merge Date**: [YYYY-MM-DD]
**Merge Duration**: [X] hours

## Changes from Master

**Commits Merged**: [X] commits
**Files Changed**: [Y] files
- Added: [A] files
- Modified: [M] files
- Deleted: [D] files

**Key Changes**:
- [Change 1]: [Description]
- [Change 2]: [Description]
- [Change 3]: [Description]

## Documentation Impact

**Documentation Files Updated**: [X]
**Documentation Files Created**: [Y]
**Code Review Phases Created**: [Z]

**Files Updated**:
- `docs/[file1].md` - [Change summary]
- `docs/[file2].md` - [Change summary]

**Files Created**:
- `docs/[new-file].md` - [Purpose]

**Code Review Phases**:
- Phase XX: [Description] - [Status]

## Validation Results

- ✅ Build Status: PASSED
- ✅ Documentation Links: VALID
- ✅ Code Review TODO: UPDATED
- ✅ Statistics: VERIFIED

## Effort Breakdown

| Activity | Effort (hours) |
|----------|----------------|
| Merge execution | [X]h |
| Change analysis | [Y]h |
| Documentation updates | [Z]h |
| Quality assurance | [A]h |
| **TOTAL** | **[B]h** |

## Next Steps

1. [ ] Monitor for any issues from merged changes
2. [ ] Complete any pending MEDIUM/LOW priority documentation updates
3. [ ] Schedule next master merge: [Recommended date]

---

**Protocol Completed**: [YYYY-MM-DD]
**Completed By**: [Name]
```

### 6.3 Clean Up Protocol Files

**Purpose**: Archive merge protocol artifacts

```bash
# Move protocol files to archive
mkdir -p .merge-protocol/archive/$(date +%Y-%m-%d)
mv .merge-protocol/*.txt .merge-protocol/archive/$(date +%Y-%m-%d)/
mv .merge-protocol/*.md .merge-protocol/archive/$(date +%Y-%m-%d)/

# Keep only MERGE-SUMMARY.md at root
cp .merge-protocol/archive/$(date +%Y-%m-%d)/MERGE-SUMMARY.md .merge-protocol/
```

---

## Phase 7: Push and Notify

### 7.1 Push Changes

**Purpose**: Push merged branch and documentation updates to remote

```bash
# Push current branch
git push origin $(git branch --show-current)

# If merge was significant, consider tagging
git tag -a merge-master-$(date +%Y%m%d) -m "Merged master $(git log -1 --format=%h master) into $(git branch --show-current)"
git push origin merge-master-$(date +%Y%m%d)
```

### 7.2 Notify Team (Optional)

**Purpose**: Inform team of significant changes

**Notification Template**:

```
Subject: Master merged into [branch-name] - Documentation Updated

Hi Team,

I've merged the latest master branch into [branch-name] and updated documentation to reflect the changes.

**Merge Summary**:
- Master Commit: [hash]
- Commits Merged: [X]
- Files Changed: [Y]

**Documentation Updates**:
- [File 1]: [Change summary]
- [File 2]: [Change summary]

**Key Changes from Master**:
- [Change 1]
- [Change 2]

**Validation**:
✅ Build passing
✅ Tests passing
✅ Documentation updated

**Next Actions**:
- Review updated documentation in docs/
- Check code review Phase XX in docs/code-review/

Full merge summary: .merge-protocol/MERGE-SUMMARY.md

[Your Name]
```

---

## Troubleshooting

### Issue: Merge Conflicts Too Complex

**Symptom**: Too many conflicts to resolve manually

**Solution**:
```bash
# Abort merge
git merge --abort

# Try merge with strategy
git merge master -X theirs  # Prefer master changes
# OR
git merge master -X ours    # Prefer branch changes

# OR use interactive merge tool
git mergetool
```

### Issue: Build Breaks After Merge

**Symptom**: Solution won't build after merge

**Solution**:
```bash
# Clean build artifacts
dotnet clean

# Restore packages
dotnet restore

# Rebuild
dotnet build

# If still failing, check for:
# - Missing package versions
# - Incompatible changes
# - Need to update project references
```

### Issue: Documentation Scope Too Large

**Symptom**: Too many documentation updates needed

**Solution**:
1. Create Code Review Phase document instead of updating immediately
2. Break into sprints (Immediate, Short-term, Long-term)
3. Mark CRITICAL items only for immediate update
4. Schedule rest for later

### Issue: Unclear What Documentation Needs Update

**Symptom**: Can't determine documentation impact

**Solution**:
1. Use `git diff master HEAD` to see all changes
2. Focus on public API changes (.cs files with `public` methods)
3. Focus on UI changes (.tsx/.jsx files)
4. When in doubt, create a Code Review Phase for analysis

---

## Success Metrics

This protocol is successful when:

- ✅ **Merge Completes**: Branch successfully merged with master
- ✅ **Build Passes**: Solution builds without errors
- ✅ **Docs Updated**: All affected documentation updated
- ✅ **Code Review Current**: TODO.md reflects new phases if needed
- ✅ **Audit Trail**: MERGE-SUMMARY.md documents the merge
- ✅ **Team Informed**: Relevant stakeholders notified

---

## Appendix A: Quick Reference Commands

```bash
# Phase 1: Preparation
git fetch origin master
git branch --show-current
mkdir -p .merge-protocol

# Phase 2: Merge
git merge master
git status
git diff --stat master HEAD

# Phase 3: Analysis
git log --oneline HEAD~1..master
git diff --name-status master HEAD

# Phase 4: Documentation
# [Edit docs files]
git add docs/
git commit -m "docs: update for master merge"

# Phase 5: Validation
dotnet build
grep -r "TODO" docs/

# Phase 6: Commit
git log -1 --pretty=fuller

# Phase 7: Push
git push origin $(git branch --show-current)
```

---

## Appendix B: File Structure

```
.merge-protocol/
├── MERGE-SUMMARY.md              # Final summary (keep)
├── archive/
│   └── YYYY-MM-DD/               # Archive per merge date
│       ├── pre-merge-commit.txt
│       ├── pre-merge-files.txt
│       ├── post-merge-commit.txt
│       ├── merged-commits.txt
│       ├── change-categories.md
│       ├── change-intent.md
│       └── doc-impact.md
└── README.md                     # Protocol usage notes

docs/
├── code-review/
│   ├── TODO.md                   # Updated with new phases
│   ├── XX-merge-documentation.md # New phase (if warranted)
│   └── README.md                 # Updated if critical issues
├── [other-docs]/
│   └── [updated-files].md        # Updated documentation files
└── prompts/
    └── code-review-gap-analysis-and-execution.md  # Reference
```

---

## Version History

| Version | Date | Changes | Author |
|---------|------|---------|--------|
| 1.0 | 2025-11-21 | Initial protocol creation | Claude |

---

**For AI Assistants**: This protocol should be executed when:
1. User requests to merge master into their branch
2. Master merge is needed before documentation updates
3. Documentation is out of sync after merge
4. Code review gap analysis is needed post-merge

Use TodoWrite tool to track progress through all 7 phases.
