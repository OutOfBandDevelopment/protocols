# Documentation Branch Master Merge Protocol - Quick Start

## Overview

This protocol is specifically designed for **documentation branches** (like `users/mattw/claude-docs`). It automates merging `master` into your documentation branch and identifies which **code changes from master** require documentation updates.

**Key Concept**: You're working on a docs-only branch. When you merge master, you get code changes that others made. This protocol helps you identify what code changed so you can update documentation accordingly.

## Files

| File | Purpose |
|------|---------|
| `MERGE-AND-DOC-UPDATE-PROTOCOL.md` | Full detailed protocol (7 phases) |
| `execute-merge-protocol.ps1` | PowerShell automation script |
| `MERGE-PROTOCOL-README.md` | This quick start guide |

## Important: This is for Documentation Branches Only

✅ **Use this protocol when**:
- Your branch is named like `users/*/claude-docs`, `docs/*`, or similar
- Your branch contains primarily documentation changes (`.md` files)
- You need to merge master to get code changes others made
- You need to update docs based on those code changes

❌ **Don't use this protocol for**:
- Feature branches with code changes
- Regular master → feature branch merges
- Code-heavy branches

## Real-World Example

**Scenario**: You're on branch `users/mattw/claude-docs` working on documentation. Meanwhile, other developers have pushed code changes to `master`. You need to:
1. Merge master into your docs branch to stay current
2. Identify what CODE changed in master
3. Update docs to reflect those code changes

**Workflow**:
```powershell
# You're on users/mattw/claude-docs (docs-only branch)
# Other devs pushed 10 commits to master with code changes

# Run the protocol script
.\protocols\execute-merge-protocol.ps1

# Script output shows:
#   - Master has 10 new commits
#   - 15 .cs files changed (backend code)
#   - 5 .tsx files changed (UI code)
#   - 2 .sql files changed (database)

# Script creates:
#   .merge-protocol/code-changes-to-document.md  (lists the 22 code files)
#   .merge-protocol/documentation-todo.md         (tells you which docs to update)

# You review the code changes:
code .merge-protocol/code-changes-to-document.md

# You see: PatientService.cs added new public method GetPatientHistory()
# You update docs:
code docs/api/patient-service.md  # Add docs for new method

# Commit:
git add docs/api/patient-service.md
git commit -m "docs: document GetPatientHistory method added in master"
git push
```

## Quick Start

### Option 1: Automated Execution (Recommended)

```powershell
# From your documentation branch (e.g., users/mattw/claude-docs)
.\protocols\execute-merge-protocol.ps1

# Dry run (preview what CODE changes are in master, no actual merge)
.\protocols\execute-merge-protocol.ps1 -DryRun

# Skip build validation (faster)
.\protocols\execute-merge-protocol.ps1 -SkipBuild

# Force execution even if branch name doesn't look like a docs branch
.\protocols\execute-merge-protocol.ps1 -Force
```

**What the script does**:
1. ✅ Verifies you're on a documentation branch
2. ✅ Updates local master from remote
3. ✅ **Analyzes CODE changes in master** (not doc changes)
4. ✅ Merges master into your docs branch
5. ✅ **Creates list of code files changed** (`.cs`, `.tsx`, `.sql`, etc.)
6. ✅ **Maps code changes to documentation needs**
7. ✅ Validates build (optional)
8. ✅ Creates documentation TODO list

**Output files** (in `.merge-protocol/`):
- `MERGE-SUMMARY.md` - Executive summary of merge
- `code-changes-to-document.md` - **List of CODE files changed in master**
- `documentation-todo.md` - **What docs need updates based on code changes**
- `master-code-changes.txt` - Raw list of code commits from master

### Option 2: Manual Execution

Follow the detailed protocol in `MERGE-AND-DOC-UPDATE-PROTOCOL.md`:

```bash
# Phase 1: Preparation
git fetch origin master
mkdir -p .merge-protocol

# Phase 2: Merge
git merge master

# Phase 3-7: Follow protocol document
```

## After Merge: Documentation Updates

Once merge is complete, follow these steps:

### Step 1: Review CODE Changes from Master

```powershell
# Read the merge summary
code .merge-protocol/MERGE-SUMMARY.md

# IMPORTANT: Review what CODE changed in master
code .merge-protocol/code-changes-to-document.md

# Review the documentation TODO
code .merge-protocol/documentation-todo.md
```

### Step 2: Understand What CODE Changed

The key file is `code-changes-to-document.md`. It shows:
- **Backend changes**: `.cs` files modified in master
- **Frontend changes**: `.tsx`/`.jsx` files modified in master
- **Database changes**: `.sql` files modified in master
- **Build/config changes**: `.csproj`/`.sln` files modified in master

For each code file, ask yourself:
- ❓ Does this change public APIs?
- ❓ Does this affect user-visible behavior?
- ❓ Is this a security/compliance fix?
- ❓ Does this change how developers build/run the app?

### Step 3: Prioritize Documentation Updates

Based on `documentation-todo.md`:

- **CRITICAL Priority**: Public API changes, breaking changes, security fixes
- **HIGH Priority**: New user-visible features, database schema changes
- **MEDIUM Priority**: Bug fixes with behavior changes, minor API updates
- **LOW Priority**: Internal refactors, no user-visible changes

### Step 4: Update Documentation Based on Code Changes

For **small changes** (1-3 code files changed, < 2 hours):
```bash
# Review the specific code file that changed
code [path/to/changed/file.cs]

# Understand what changed in that code file
git diff master HEAD -- [path/to/changed/file.cs]

# Update affected documentation
code docs/[affected-file].md

# Commit updates with reference to code changes
git add docs/
git commit -m "docs: update [doc-file] to reflect [code-file] changes from master"
```

**Example**:
```bash
# Master changed Source/API/PatientService.cs
# Review what changed:
git diff master HEAD -- Source/API/PatientService.cs

# If it added a new public method, update API docs:
code docs/api/patient-service.md
# Add documentation for the new method

git add docs/api/patient-service.md
git commit -m "docs: document new GetPatientHistory method in PatientService"
```

For **large changes** (5+ code files, > 2 hours, security/compliance impact):

1. **Create Code Review Phase** following the template in `MERGE-AND-DOC-UPDATE-PROTOCOL.md` Phase 4.2

2. **Update TODO.md**:
   ```bash
   code docs/code-review/TODO.md
   ```

   Add new phase:
   ```markdown
   ### Phase XX: Master Merge Code Changes Documentation 🔴
   **Status**: Pending
   **Priority**: CRITICAL/HIGH
   **Estimated Effort**: X days
   **Trigger**: Master merge [date] - [X] code files changed

   - [ ] **XX.1 Analyze Code Changes** - Review [X] .cs/.tsx files from master
   - [ ] **XX.2 Map to Documentation** - Identify which docs need updates
   - [ ] **XX.3 Update Documentation** - Create/update affected doc files
   - [ ] **XX.4 Validation** - Verify accuracy with code examples

   **Rationale**: Master merge brought in [X] code changes affecting [API/UI/database].
   Code files: [list key files from code-changes-to-document.md]
   ```

3. **Create Phase Document** (if critical):
   ```bash
   code docs/code-review/XX-master-merge-code-documentation.md
   ```

   Use the template from `MERGE-AND-DOC-UPDATE-PROTOCOL.md` Section 4.2

### Step 4: Validate and Commit

```bash
# Validate links
grep -r "](docs/" docs/ | sed 's/.*](\(docs\/[^)]*\)).*/\1/' | while read link; do
    [ -f "$link" ] || echo "Broken link: $link"
done

# Check for TODOs left behind
grep -r "TODO\|FIXME" docs/

# Commit all documentation changes
git add docs/
git commit -m "docs: update documentation for master merge $(git log -1 --format=%h master)"

# Push
git push origin $(git branch --show-current)
```

## Use Cases

### Use Case 1: Regular Master Sync

**Scenario**: Weekly/bi-weekly sync with master, minimal changes expected

**Approach**:
```powershell
# Execute automated script
.\protocols\execute-merge-protocol.ps1

# Review MERGE-SUMMARY.md
code .merge-protocol/MERGE-SUMMARY.md

# Update 1-3 doc files if needed
code docs/[file].md

# Commit and push
git add docs/
git commit -m "docs: update for master sync"
git push
```

**Time**: 15-30 minutes

### Use Case 2: Major Feature Merge

**Scenario**: Master has significant new features, breaking changes, or security updates

**Approach**:
```powershell
# Execute automated script
.\protocols\execute-merge-protocol.ps1

# Review ALL generated files
code .merge-protocol/doc-impact.md
code .merge-protocol/change-categories.md

# Create Code Review Phase
code docs/code-review/TODO.md  # Add new phase
code docs/code-review/XX-merge-doc-update.md  # Create phase document

# Update multiple documentation files
# [Follow Phase 4 of full protocol]

# Commit with detailed message
git add docs/
git commit -m "docs: comprehensive update for major master merge - [summary]"
git push
```

**Time**: 2-8 hours

### Use Case 3: Merge Conflicts

**Scenario**: Merge has conflicts that need manual resolution

**Approach**:
```powershell
# Attempt merge
.\protocols\execute-merge-protocol.ps1

# If conflicts detected:
# 1. Script will list conflicted files
# 2. Manually resolve each conflict:
code [conflicted-file]

# 3. Stage resolved files
git add [resolved-file]

# 4. Complete merge
git commit -m "Merge master into $(git branch --show-current) - resolved conflicts"

# 5. Re-run analysis
.\protocols\execute-merge-protocol.ps1 -SkipBuild

# 6. Continue with documentation updates
```

**Time**: 30 minutes - 2 hours (depends on conflicts)

### Use Case 4: Documentation-Only Changes

**Scenario**: Master has only documentation changes, no code changes

**Approach**:
```powershell
# Execute script
.\protocols\execute-merge-protocol.ps1

# Review doc-impact.md (will show only .md files changed)
code .merge-protocol/doc-impact.md

# Review changed docs for consistency
git diff master HEAD -- '*.md'

# Update cross-references if needed
# No new code review phase needed

# Commit
git add docs/
git commit -m "docs: sync documentation with master"
git push
```

**Time**: 10-20 minutes

## Common Scenarios and Solutions

### Scenario: "I just want to merge master"

```powershell
.\protocols\execute-merge-protocol.ps1
```

That's it! The script handles everything and tells you what documentation needs updating.

### Scenario: "I want to see what would change before merging"

```powershell
.\protocols\execute-merge-protocol.ps1 -DryRun
```

### Scenario: "Merge succeeded but I don't know what docs to update"

```powershell
# Review the doc impact file
code .merge-protocol/doc-impact.md

# It will tell you exactly which docs might need updates based on file types changed
```

### Scenario: "Merge succeeded but build failed"

```powershell
# Review build errors
dotnet build

# Fix code issues first
# Then update documentation
# Documentation should reflect the FIXED code, not the broken merge
```

### Scenario: "Too many changes, overwhelmed"

```powershell
# Create a Code Review Phase instead of updating docs immediately
code docs/code-review/TODO.md

# Add a new phase to track the work
# Break it into smaller tasks
# Follow the template in MERGE-AND-DOC-UPDATE-PROTOCOL.md Phase 4.2
```

## Troubleshooting

### Issue: "Script says uncommitted changes"

**Solution**:
```bash
# Commit your changes first
git add .
git commit -m "WIP: your changes"

# Or stash them
git stash

# Then run merge script
.\protocols\execute-merge-protocol.ps1

# If you stashed, restore after merge
git stash pop
```

### Issue: "Cannot merge master into itself"

**Solution**:
```bash
# You're on master branch, switch to your feature branch
git checkout users/[your-name]/[branch-name]

# Then run merge script
.\protocols\execute-merge-protocol.ps1
```

### Issue: "Merge conflicts, don't know how to resolve"

**Solution**:
1. Open conflicted file in VS Code (has good conflict resolution UI)
2. For each conflict, choose:
   - **Accept Current**: Keep your branch's changes
   - **Accept Incoming**: Keep master's changes
   - **Accept Both**: Include both changes
   - **Manual**: Edit to create correct version
3. Save file
4. `git add [file]`
5. `git commit`

### Issue: "Build fails after merge"

**Solution**:
```bash
# Clean and rebuild
dotnet clean
dotnet restore
dotnet build

# If still failing:
# 1. Check for package version conflicts in .csproj files
# 2. Check for breaking API changes from master
# 3. May need to update code to match master's changes
```

### Issue: "Documentation scope is huge"

**Solution**:
- **Don't try to update everything at once**
- **Create a Code Review Phase** to track the work
- **Break into sprints**: Immediate, Short-term, Long-term
- **Focus on CRITICAL first**: Breaking changes, security, public APIs
- **Schedule the rest**: Medium/Low priority can wait

## Integration with Code Review Process

This merge protocol integrates with the existing code review process:

### When Merge → Code Review Phase

Create a new Code Review Phase when:
- ✅ Master merge affects **3+ documentation files**
- ✅ Master merge introduces **breaking changes**
- ✅ Master merge adds **new security/compliance requirements**
- ✅ Documentation updates estimated **> 4 hours**

### When Merge → Direct Doc Update

Update documentation directly when:
- ✅ Master merge affects **1-2 documentation files**
- ✅ Changes are **non-breaking**
- ✅ Documentation updates estimated **< 2 hours**
- ✅ Changes are **minor clarifications**

### Workflow Integration

```
Master Merge
    ↓
Execute Merge Protocol Script
    ↓
Review MERGE-SUMMARY.md
    ↓
    ├─→ Small Changes (< 2h)
    │       ↓
    │   Direct Doc Update
    │       ↓
    │   Commit & Push
    │
    └─→ Large Changes (> 2h)
            ↓
        Create Code Review Phase
            ↓
        Add to docs/code-review/TODO.md
            ↓
        Create Phase Document (XX-*.md)
            ↓
        Execute Phase (follow code-review-gap-analysis-and-execution.md)
            ↓
        Mark Complete in TODO.md
            ↓
        Commit & Push
```

## Best Practices

### DO ✅

- ✅ Run merge protocol script before manually merging
- ✅ Review MERGE-SUMMARY.md after every merge
- ✅ Update docs immediately for CRITICAL changes (security, breaking)
- ✅ Create Code Review Phase for large documentation efforts
- ✅ Validate build after merge before updating docs
- ✅ Keep .merge-protocol/ files for audit trail
- ✅ Commit documentation separately from code
- ✅ Use descriptive commit messages referencing merge commit

### DON'T ❌

- ❌ Skip documentation updates ("I'll do it later")
- ❌ Update docs before merge completes successfully
- ❌ Update docs before build passes
- ❌ Commit documentation and code together (separate commits)
- ❌ Delete .merge-protocol/ files (archive them)
- ❌ Force push after merge (breaks history)
- ❌ Merge master multiple times before updating docs

## Metrics and Success

Track your merge protocol success:

```markdown
# Merge Protocol Metrics

## Last 5 Merges

| Date | Branch | Commits | Files | Doc Files Updated | Time | Issues |
|------|--------|---------|-------|-------------------|------|--------|
| [date] | [branch] | X | Y | Z | Xh | None |
| [date] | [branch] | X | Y | Z | Xh | 2 conflicts |
...
```

**Success Criteria**:
- ✅ Merge completes without build breaks
- ✅ Documentation updated within 24 hours
- ✅ No broken links introduced
- ✅ No TODOs left in documentation
- ✅ Team notified of significant changes

## Getting Help

### Protocol Issues

- **Full Protocol**: Read `MERGE-AND-DOC-UPDATE-PROTOCOL.md` for detailed guidance
- **Script Issues**: Review script output, check PowerShell version (5.1+)
- **Merge Conflicts**: See "Troubleshooting" section above

### Code Review Process

- **Code Review Protocol**: See `CODE-REVIEW-PROTOCOL.md`
- **Gap Analysis**: See `docs/prompts/code-review-gap-analysis-and-execution.md`
- **TODO Tracking**: See `docs/code-review/TODO.md`

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-11-21 | Initial protocol and script creation |

---

**Quick Reference Card**

```bash
# Merge master into current branch
.\protocols\execute-merge-protocol.ps1

# After merge, review summary
code .merge-protocol/MERGE-SUMMARY.md
code .merge-protocol/doc-impact.md

# Update docs
code docs/[file].md

# Commit
git add docs/ && git commit -m "docs: update for master merge"

# Push
git push
```

---

**Protocol Maintained By**: Engineering Team
**Last Updated**: 2025-11-21
