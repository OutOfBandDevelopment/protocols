# Code Change Review Protocol

## Trigger Phrases
- "review my changes"
- "review this change"
- "code review"
- "review my code"
- "check my changes"

## Overview

This protocol provides a comprehensive code review process for changes in the CadLink codebase. It ensures all changes are thorough, consistent, and complete across the entire application.

**Prerequisites:**
- Feature documentation must exist in `docs/notes/`
- If documentation doesn't exist, run "create documentation" first

**What This Protocol Does:**
- Reviews all code changes
- Searches entire codebase for missed changes
- Updates existing documentation with Change Review Summary
- Does NOT create new documentation (use "create documentation" for that)

## Protocol Steps

### 1. Verify Documentation Exists

**Actions:**
- [ ] Check current git branch
- [ ] Search for feature documentation in `docs/notes/`
- [ ] Verify documentation file exists
- [ ] Verify documentation has standard structure

**Commands:**
```bash
# Check branch
git branch --show-current

# Search for documentation
ls docs/notes/*.md 2>/dev/null
```

**Documentation Search Strategy:**
1. Look for files matching branch name pattern
2. Look for files matching work item number
3. List all documentation and ask user which one

**If No Documentation Found:**
```
❌ PREREQUISITE MISSING: Feature Documentation Required

Before running code review, you must create feature documentation.

Please run: "create documentation"

This will create the foundation documentation in docs/notes/ that the
code review will update with the Change Review Summary.

After creating documentation, run this protocol again.
```

**If Multiple Documentation Files Found:**
```
ℹ️ Multiple documentation files found:

1. docs/notes/{file1}.md
2. docs/notes/{file2}.md
3. docs/notes/{file3}.md

Which documentation file should be updated with the review?
Enter number (1-3):
```

**Output:**
```
✓ Documentation found: docs/notes/{filename}.md
✓ Ready to proceed with code review
```

---

### 2. Identify Current Context

**Actions:**
- [ ] Check current git branch
- [ ] Check git status for modified, added, deleted files
- [ ] Check for untracked files that should be included
- [ ] Identify uncommitted vs committed changes

**Commands:**
```bash
git branch --show-current
git status --short
git diff --stat
git log --oneline -10
```

**Output:** Summary of current branch and changed files

---

### 2. Validate Branch Name

**Actions:**
- [ ] Check if branch name follows convention
- [ ] Extract work item number from branch name if present
- [ ] Verify branch name is descriptive

**Branch Naming Conventions:**
- `features/{workitem}-{description}` - New features
- `bugs/{workitem}-{description}` - Bug fixes
- `support/{version}` - Support/patch branches
- `users/{username}/{description}` - Personal/experimental branches

**Validation Rules:**
- Branch should contain work item number (if applicable)
- Description should be kebab-case
- Should be concise but descriptive

**If Invalid:**
- Suggest a better branch name
- Explain the naming convention

---

### 3. Determine Work Item

**Actions:**
- [ ] Extract work item number from branch name
- [ ] If not found, search commit messages for work item references
- [ ] If still not found, ask user for work item number
- [ ] If no work item exists, note this for documentation

**Extraction Patterns:**
- Branch name: `features/115163-plaintext-password-fix` → 115163
- Commit message: `#115163` or `work item 115163` → 115163
- Merge commit: Look for work item in merge message

**If No Work Item Found:**
```
I couldn't find a work item associated with these changes.

Options:
1. Provide the work item number (e.g., "115163")
2. This is experimental work without a work item
3. This should have a work item but doesn't yet

Which applies to your changes?
```

---

### 4. Review Work Item Details

**Actions:**
- [ ] Fetch work item from Azure DevOps
- [ ] Review title and description
- [ ] Check acceptance criteria
- [ ] Review related work items
- [ ] Check comments for additional context
- [ ] Note any linked pull requests or branches

**Azure DevOps Queries:**
```javascript
mcp__azure-devops__wit_get_work_item(id, project: "CadLink", expand: "relations")
mcp__azure-devops__wit_list_work_item_comments(project: "CadLink", workItemId)
```

**Extract:**
- Work item title
- Description and acceptance criteria
- Related work items (parent, children, related)
- Comments that provide implementation guidance
- Any special requirements or constraints

**Output:** Summary of work item requirements

---

### 5. Analyze Changed Files

**Actions:**
- [ ] Review each modified file
- [ ] Identify the type of changes (bug fix, feature, refactor, etc.)
- [ ] Check for common issues:
  - Hardcoded values that should be configurable
  - Missing error handling
  - Security vulnerabilities (SQL injection, XSS, command injection, etc.)
  - Memory leaks or resource disposal issues
  - Thread safety issues
  - Performance concerns
- [ ] Verify coding standards compliance
- [ ] Check for TODO/FIXME/HACK comments

**File Categories:**
1. **Core Logic** - Business logic changes
2. **Data Access** - Database/repository changes
3. **UI** - User interface changes
4. **Configuration** - Settings/config changes
5. **Infrastructure** - Build, deployment, installer changes
6. **Tests** - Test additions/modifications
7. **Documentation** - README, comments, docs

**For Each File:**
- Purpose of changes
- Risk level (Low/Medium/High)
- Potential side effects
- Dependencies on other components

---

### 6. Search for Missed Changes

**Critical:** Search the entire codebase for related code that might need similar changes.

#### 6.1 Search by Pattern

**Actions:**
- [ ] Identify key classes, methods, or patterns changed
- [ ] Search for similar usages across the codebase
- [ ] Check if the same pattern exists elsewhere

**Example Scenarios:**

**Password Handling:**
If changing password handling in one place:
```bash
# Search for other password parameters in Process.Start
grep -r "Process\.Start" --include="*.cs" | grep -i "password"

# Search for similar encryption operations
grep -r "KeyDatabaseLegacy" --include="*.cs"
```

**Database Operations:**
If changing database access:
```bash
# Search for similar database operations
grep -r "ExecuteNonQuery\|ExecuteReader\|ExecuteScalar" --include="*.cs"

# Search for transaction handling
grep -r "BeginTransaction" --include="*.cs"
```

**Configuration Changes:**
If adding configuration:
```bash
# Search for configuration reading
grep -r "ConfigurationManager\|appsettings\|\.config" --include="*.cs" --include="*.config"
```

#### 6.2 Search by Component Type

**Actions:**
- [ ] Check if similar components exist (services, tools, utilities)
- [ ] Verify consistency across component types

**Component Patterns to Check:**

**Runtime vs Installer vs Tools:**
```
Source/CadLink.Common/                 ← Runtime usage
Source/Tools/DbKeyCustomAction/        ← Installer usage
InternalTools/EncryptionManager/       ← Tool usage
```

**Server vs Client:**
```
Source/CadLink.Server/                 ← Server-specific
Source/CadLink.Client/                 ← Client-specific
Source/CadLink.Common/                 ← Shared
```

**Search Commands:**
```bash
# Find similar files
find . -name "*{ChangedFileName}*" -type f

# Find usages of changed class
grep -r "class_name" --include="*.cs"

# Find similar functionality
grep -r "method_pattern" --include="*.cs" -A 5
```

#### 6.3 Check Related Systems

**Actions:**
- [ ] Tests - Unit tests, integration tests, automated tests
- [ ] Installers - Server installer, client installer (32/64 bit)
- [ ] Configuration - Settings files, config templates
- [ ] Documentation - README files, user guides, API docs
- [ ] Build System - Project files, build scripts, CI/CD

**Test Locations:**
```
Tests/CadLink.Tests/
Tests/CadLinkAutomatedTests/
Tests/IntegrationTests/
```

**Installer Locations:**
```
Source/Tools/Installers/CadLinkServerInstaller/
Source/Tools/Installers/CadLinkClientInstaller/
```

**Configuration Locations:**
```
Source/CadLink.Server/App.config
Source/CadLink.Client/App.config
**/appsettings.json
**/*Settings.xml
```

#### 6.4 Identify Missed Changes

**Output:**
For each potential missed location:
```
⚠️ POTENTIAL MISSED CHANGE:

Location: Source/Tools/DbKeyCustomAction/CustomAction.cs:154
Current code: process.StartInfo = new ProcessStartInfo(..., password)
Reason: Similar password handling pattern not updated
Recommendation: Apply same environment variable fix
Priority: HIGH
```

---

### 7. Review Code Quality

**Actions:**
- [ ] Check for code duplication
- [ ] Verify error handling
- [ ] Review logging and diagnostics
- [ ] Check resource management (using statements, disposal)
- [ ] Verify input validation
- [ ] Check for security issues

**Security Checklist:**
- [ ] SQL Injection - Parameterized queries used?
- [ ] XSS - Output encoding used?
- [ ] Command Injection - Safe command execution?
- [ ] Path Traversal - Path validation used?
- [ ] Sensitive Data - Passwords/secrets not logged or exposed?
- [ ] Authentication/Authorization - Proper checks in place?

**Code Quality Checklist:**
- [ ] Single Responsibility - Methods do one thing?
- [ ] DRY Principle - No unnecessary duplication?
- [ ] Naming - Clear, descriptive names?
- [ ] Magic Numbers - Constants instead of literals?
- [ ] Comments - Complex logic documented?
- [ ] Error Messages - User-friendly and informative?

---

### 8. Verify Test Coverage

**Actions:**
- [ ] Check if tests exist for changed code
- [ ] Verify tests cover new functionality
- [ ] Check if tests cover error cases
- [ ] Run existing tests if possible
- [ ] Identify gaps in test coverage

**Test Types to Consider:**
1. **Unit Tests** - Individual component testing
2. **Integration Tests** - Component interaction testing
3. **Automated Tests** - End-to-end scenarios
4. **Manual Tests** - User acceptance testing

**Test Checklist:**
- [ ] Happy path tested?
- [ ] Error cases tested?
- [ ] Edge cases tested?
- [ ] Boundary conditions tested?
- [ ] Concurrent access tested (if applicable)?
- [ ] Backward compatibility tested?

**If Tests Missing:**
```
⚠️ TEST COVERAGE GAP:

Component: LegacySupport.DecryptDatabase
Changes: Added environment variable for password
Missing Tests:
- Test password passed via environment variable
- Test process exits cleanly
- Test error handling when env var not set
Recommendation: Add unit tests before merging
Priority: HIGH
```

---

### 8.1 Check for Test Results

**Actions:**
- [ ] Look for test plan in `docs/testing/`
- [ ] Check if test plan has been executed
- [ ] Review test results if available
- [ ] Check for test summary in feature documentation
- [ ] Verify pass/fail status

**Search Locations:**
```bash
# Check for test plan
ls docs/testing/{matching-filename}.md

# Look for test results files
find . -name "*TestResults*" -o -name "*test-results*"

# Check for test output
ls TestResults/ 2>/dev/null
```

**Test Plan Status Check:**

Read test plan file (if exists) and check for:
- Test execution status in tables
- "Status" column values (⏳ Not Started, 🔄 In Progress, ✅ Passed, ❌ Failed)
- Defect table entries
- Test summary section

**Feature Documentation Check:**

Look in feature docs for "Testing" section:
- Test Plan Status (⏳ Not Created, 🔄 In Progress, ✓ Complete)
- Test execution results
- Any test summary information

**Output Scenarios:**

**Scenario 1: Tests Not Run**
```
⚠️ TESTS NOT EXECUTED

Test Plan: docs/testing/{filename}.md
Status: Created but not executed

Evidence:
- All test cases show "⏳ Not Started"
- No test results found
- Feature documentation shows "Test Plan Status: ⏳ Not Created" or "🔄 In Progress"

Recommendation:
- Run automated tests before merging
- Execute manual test procedures
- Update test plan with results
- Update feature documentation with test summary

Priority: CRITICAL (tests must pass before merge)
```

**Scenario 2: Tests Partially Run**
```
⚠️ INCOMPLETE TEST EXECUTION

Test Plan: docs/testing/{filename}.md
Status: Partially executed

Evidence:
- {count} tests passed (✅)
- {count} tests failed (❌)
- {count} tests not started (⏳)
- {count} tests in progress (🔄)

Defects Found: {count}

Recommendation:
- Complete remaining tests
- Fix failing tests or document as known issues
- Resolve all critical defects before merging

Priority: HIGH
```

**Scenario 3: Tests Passed**
```
✓ TESTS EXECUTED AND PASSED

Test Plan: docs/testing/{filename}.md
Status: Complete

Evidence:
- {count} tests passed (✅)
- 0 tests failed
- All critical test cases passed
- All acceptance criteria validated

Test Summary:
- Unit Tests: {count} passed
- Integration Tests: {count} passed
- Manual Tests: {count} passed
- Security Tests: {count} passed (if applicable)
- Performance Tests: {count} passed (if applicable)

Defects: {count} (all resolved or low priority)

Status: APPROVED - Ready for merge
```

**Scenario 4: Tests Failed**
```
❌ TESTS FAILED

Test Plan: docs/testing/{filename}.md
Status: Tests executed with failures

Evidence:
- {count} tests passed (✅)
- {count} tests failed (❌)
- {count} critical defects logged

Failed Tests:
- TC-{number}: {test case name} - {reason}
- TC-{number}: {test case name} - {reason}

Critical Defects:
- #{defect id}: {description}

Recommendation:
- Fix all failing tests before merging
- Resolve all critical defects
- Re-run tests after fixes
- Update test plan with new results

Priority: CRITICAL - DO NOT MERGE until resolved
```

**Scenario 5: No Test Plan**
```
⚠️ NO TEST PLAN FOUND

Expected: docs/testing/{filename}.md
Found: None

Recommendation:
- Run "create test plan" protocol first
- Execute tests according to plan
- Update results in test plan
- Then re-run code review

Priority: HIGH - Test plan required before merge
```

**Include in Change Review Summary:**

Add "Test Execution Status" subsection to Change Review Summary:
```markdown
### Test Execution Status

**Test Plan:** {docs/testing/{filename}.md}
**Status:** {Not Created|Created but Not Run|In Progress|Failed|Passed}

**Test Results:**
- Total Test Cases: {count}
- Passed: {count} (✅)
- Failed: {count} (❌)
- In Progress: {count} (🔄)
- Not Started: {count} (⏳)

**Automated Tests:**
- Unit Tests: {pass/fail count}
- Integration Tests: {pass/fail count}

**Manual Tests:**
- Executed: {count}
- Pending: {count}

**Defects:**
- Critical: {count}
- High: {count}
- Medium: {count}
- Low: {count}

**Merge Readiness:**
- {✓|❌} All critical tests passed
- {✓|❌} All critical defects resolved
- {✓|❌} Acceptance criteria validated
```

---

### 9. Check and Update Documentation

**Actions:**
- [ ] Verify code comments are updated
- [ ] Check if README needs updates
- [ ] Verify API documentation is current
- [ ] Check if user-facing docs need updates
- [ ] Verify inline documentation explains "why" not "what"
- [ ] **Update feature documentation with Change Review Summary**

**Documentation Locations:**
```
README.md files
docs/ directory
docs/notes/ (feature documentation - REQUIRED, verified in step 1)
InternalTools/**/README.md
XML documentation comments (///)
Inline comments
```

**Documentation Checklist:**
- [ ] Breaking changes documented?
- [ ] New features documented?
- [ ] Configuration changes documented?
- [ ] Migration guide provided (if needed)?
- [ ] Known issues documented?
- [ ] Feature documentation exists (verified in step 1)?
- [ ] TODOs in feature documentation filled in?

---

#### 9.1 Update Feature Documentation with Change Review Summary

**Purpose:**
Update the existing feature documentation (verified in step 1) with comprehensive Change Review Summary.

**Important:**
- Does NOT create new documentation (use "create documentation" protocol for that)
- Updates existing documentation file found in step 1
- Replaces "Change Review Summary" placeholder section
- Updates "Last Updated" date
- Adds version history entry

**Documentation File:**
Use the documentation file identified in step 1: `docs/notes/{filename}.md`

**Steps:**

1. **Read Existing Documentation**
   - Open the documentation file from step 1
   - Locate "Change Review Summary" section
   - Verify it exists (created by "create documentation" protocol)

2. **Generate Change Review Summary Content**

**Change Review Summary Structure:**

```markdown
## Change Review Summary
**REQUIRED:** Include a comprehensive summary of the code review process

### Review Process
- What was reviewed and verified
- Search methodology used to find all related code
- Components analyzed

### Components Reviewed and Updated
- List all components touched with commit references
- Include line number references for key changes

### Security Analysis (if applicable)
- Vulnerability found (severity, attack surface, impact)
- Mitigation applied (risk reduction, protection method)

### Completeness Check
- Codebase search results
- Pattern consistency verification
- Multi-component coverage

### Issues Found and Fixed
- Document any issues discovered during review
- Show before/after code for fixes
- Include line number references

### Testing Status
- Automated tests available/required
- Manual test procedures
- Pre-merge testing recommendations

### Documentation Status
- User documentation created/updated
- Feature documentation completed
- Related documentation links

### Build Configuration
- Build dependencies identified
- Projects requiring rebuild
- Deployment requirements

### Breaking Changes Assessment
- Backward compatibility analysis
- API changes (if any)
- Migration requirements

### Work Item Integration
- Work item reference and status
- Related work items

### Overall Assessment
- Review status and completion
- Key strengths of the implementation
- Recommendations before merge
- Priority level
- Merge readiness

## Problem Statement
What problem does this solve? What was broken/missing?

## Solution
How does the change address the problem?

## Changes Made
### Files Modified
List all changed files with:
- File path
- Purpose of changes
- Before/after code examples (if significant)

### Components Affected
- Runtime components
- Installer components
- Internal tools
- Tests
- Configuration

## Impact Analysis
- What functionality is affected?
- Are there breaking changes?
- Performance implications?
- Security implications?

## Testing
- How to test the changes
- Test scenarios covered
- Manual test procedures
- Automated test additions

## Deployment
- Build requirements
- Deployment steps
- Rollback procedures

## Use Cases / Examples
Real-world scenarios showing how the change is used

## Related Documentation
Links to:
- README files
- Test documentation
- Related features
- Azure DevOps work items

## Troubleshooting
Common issues and solutions
```

3. **Update Documentation File**

**Using Edit Tool:**
- Read the existing documentation file
- Find the "Change Review Summary" section
- Replace the placeholder content with actual review results
- Update "Last Updated" date
- Add entry to version history table

**Find and Replace:**
```markdown
OLD:
## Change Review Summary

**Note:** This section will be populated when "review my changes" protocol is run.

**Status:** ⏳ Pending Review

---

NEW:
## Change Review Summary

**Review Date:** {date}
**Reviewed By:** Claude (AI Code Assistant)
**Review Type:** Comprehensive multi-component review

{Insert full Change Review Summary content from step 2}

---
```

**Update Metadata:**
```markdown
# At top of document, update:
**Last Updated:** {current date}

# At bottom of document, add version history entry:
| {version} | {date} | {author} | Change Review Summary added |
```

4. **Documentation Quality Checklist**

- [ ] **Complete** - All sections filled out
- [ ] **Change Review Summary Included** - Comprehensive review details documented
- [ ] **Accurate** - Code references correct with line numbers
- [ ] **Clear** - Non-technical people can understand overview
- [ ] **Examples** - Includes working code examples
- [ ] **Searchable** - Includes relevant keywords
- [ ] **Self-Contained** - Can be understood without reading code
- [ ] **Linked** - References to related docs and code
- [ ] **Versioned** - Includes dates and version information
- [ ] **Review Status** - Overall assessment and merge readiness documented

5. **Special Documentation Cases**

**Security Changes:**
- **REQUIRED:** Detailed security analysis in Change Review Summary
- Include risk assessment (before/after)
- Document attack vectors mitigated
- Provide security validation steps
- Include verification commands
- Document severity, attack surface, and impact
- Show risk reduction achieved

**Breaking Changes:**
- Update existing documentation with migration guide
- Add before/after comparison
- Add compatibility notes
- Add rollback procedures

**Multi-Component Changes:**
- **REQUIRED:** Completeness check in Change Review Summary
- Document all affected components
- Show how components interact
- Include deployment order
- Note dependencies
- List all locations where pattern was applied
- Show codebase search methodology used

**Internal Tools:**
- Update with usage instructions
- Add command-line examples
- Document configuration options
- Add troubleshooting guide

6. **Update Documentation During Review**

**Process:**
1. Analyze all changes made
2. Extract key information:
   - Work item details (already in doc)
   - Changed files and purposes
   - Test coverage
   - Deployment impact
3. Read existing documentation file (from step 1)
4. Generate comprehensive Change Review Summary
5. **CRITICAL:** Replace "Change Review Summary" placeholder with:
   - Complete review process details
   - Components analyzed and updated
   - Security analysis (if applicable)
   - Completeness verification
   - Issues found and fixed
   - Testing, documentation, and build status
   - Breaking changes assessment
   - Overall assessment with merge readiness
6. Update `docs/notes/{filename}.md` (file from step 1)
7. Update "Last Updated" date and version history
8. Include documentation update in review summary

**Example Output:**
```
📄 DOCUMENTATION UPDATED:

File: docs/notes/KeyDatabaseLegacy-PasswordSecurityFix.md
Type: Security Fix Documentation
Action: Change Review Summary Added
Updated Sections: 1
Content Added:
  ✓ Change Review Summary (COMPLETE)
    - Review date and reviewer
    - Review process and methodology
    - Components reviewed (4 components)
    - Security analysis (High→Low risk)
    - Completeness check with search results
    - Issues found and fixed (2 issues)
    - Testing status and recommendations
    - Documentation status
    - Build configuration
    - Breaking changes assessment
    - Overall assessment and merge readiness
  ✓ Last Updated date updated
  ✓ Version history entry added

Documentation Status:
  ✓ Previously created by "create documentation" protocol
  ✓ Now enhanced with Change Review Summary
  ✓ Ready for "create test plan" protocol
```

---

### 10. Review Dependencies

**Actions:**
- [ ] Check for new NuGet packages
- [ ] Verify package versions are appropriate
- [ ] Check for security vulnerabilities in dependencies
- [ ] Verify licenses are acceptable
- [ ] Check for version conflicts

**Dependency Files:**
```
packages.config
*.csproj (PackageReference)
paket.dependencies
```

**Questions to Ask:**
- Are new dependencies necessary?
- Are there lighter alternatives?
- Is the package actively maintained?
- Are there known security issues?
- Does it support our target frameworks?

---

### 11. Verify Build and Deployment

**Actions:**
- [ ] Check if project files are updated
- [ ] Verify build configurations (Debug/Release)
- [ ] Check installer changes if needed
- [ ] Verify deployment scripts if applicable
- [ ] Check for breaking changes to build process

**Build Files:**
```
*.csproj
*.sln
Directory.Build.props
build scripts (.bat, .ps1)
CI/CD pipelines (.yaml)
```

**Installer Considerations:**
- New files need to be included?
- Custom actions need updates?
- Registry keys need updates?
- Permissions need changes?
- Upgrade path handled?

---

### 12. Generate Review Summary

**Actions:**
- [ ] Compile all findings
- [ ] Prioritize issues (Critical/High/Medium/Low)
- [ ] Provide actionable recommendations
- [ ] Create checklist of items to address

**Summary Template:**

```markdown
# Code Review Summary

## Change Overview
**Branch:** {branch-name}
**Work Item:** #{work-item-id} - {title}
**Change Type:** {feature|bugfix|refactor|etc}
**Files Changed:** {count}

## Work Item Alignment
✓ Changes align with work item requirements
✓ All acceptance criteria addressed
⚠️ {any gaps or concerns}

## Changed Files
1. `{file-path}` - {purpose} [{risk-level}]
2. ...

## Security Review
✓ No SQL injection risks
✓ No XSS vulnerabilities
✓ Sensitive data properly handled
⚠️ {any security concerns}

## Completeness Check
✓ All similar patterns updated
✓ Both server and client updated
✓ Installer changes applied
⚠️ {any missed locations}

### Potential Missed Changes
{list of locations that might need updates}

## Code Quality
✓ Follows coding standards
✓ Proper error handling
✓ Resource management correct
⚠️ {any quality issues}

## Test Coverage
✓ Unit tests exist
✓ Integration tests exist
⚠️ Missing test coverage for {scenarios}

## Documentation
✓ Code comments updated
✓ README updated
✓ Feature documentation generated: `docs/notes/{filename}.md`
✓ **Change Review Summary included in documentation**
⚠️ {documentation gaps}

### Documentation Updated
**File:** `docs/notes/{filename}.md`
**Type:** {Feature|Security Fix|Migration Guide|Technical Reference}
**Action:** Change Review Summary Added
**Content Updated:**
- ✓ **Change Review Summary (REQUIRED)** - Added comprehensive review details:
  - Review date and reviewer
  - Review process and methodology
  - Components reviewed and updated
  - Security analysis (if applicable)
  - Completeness check with search results
  - Issues found and fixed during review
  - Testing status and recommendations
  - Documentation status
  - Build configuration
  - Breaking changes assessment
  - Overall assessment and merge readiness
- ✓ Last Updated date updated
- ✓ Version history entry added

**Documentation Previously Created By:** "create documentation" protocol

**Documentation Now Includes:**
- ✓ Overview and problem statement
- ✓ Branch information with commits
- ✓ **Change Review Summary (COMPLETE)**
- ✓ All implementation details (to be filled by developer)
- ✓ Ready for "create test plan" protocol

**Related Documentation:**
- {link to README}
- {link to test docs}
- {link to related features}

## Recommendations

### Critical (Fix Before Merge)
1. {critical issue}

### High Priority
1. {high priority issue}

### Medium Priority
1. {medium priority issue}

### Low Priority / Nice to Have
1. {low priority suggestion}

## Approval Status
- [ ] All critical issues resolved
- [ ] High priority issues addressed
- [ ] Tests passing
- [ ] Documentation complete

**Status:** {APPROVED|CHANGES REQUESTED|NEEDS DISCUSSION}
```

---

## Usage Example

**User says:** "review my changes"

**Protocol executes:**

1. **Verify documentation exists** → Found `docs/notes/KeyDatabaseLegacy-PasswordSecurityFix.md`
2. Check git status → On branch `features/115163-plaintext-password-fix`
3. Extract work item → 115163
4. Fetch work item details from Azure DevOps
5. Review changed files:
   - `Program.cs` - Changed password handling
   - `LegacySupport.cs` - Updated password passing
6. Search for similar patterns:
   - Found: `CustomAction.cs` uses same pattern
   - Found: `MainWindow.xaml.cs` uses same pattern
   - Recommendation: Apply same fix
7. Check tests → Tests exist but need updates
8. Generate comprehensive Change Review Summary
9. **Update documentation** → Replace "Change Review Summary" section in existing doc
10. Provide recommendations

---

## Customization

### Project-Specific Patterns

Add common patterns for your codebase:

```javascript
// Password/Secret Handling
grep -r "password.*Process\.Start" --include="*.cs"
grep -r "credential.*commandline" --include="*.cs"

// Database Operations
grep -r "ExecuteNonQuery" --include="*.cs"
grep -r "SQLiteConnection" --include="*.cs"

// Configuration
grep -r "ConfigurationManager\.AppSettings" --include="*.cs"
grep -r "\.GetValue\|\.GetSection" --include="*.cs"
```

### Risk Assessment Rules

**High Risk:**
- Database schema changes
- Authentication/authorization changes
- Encryption changes
- Security-related changes
- Breaking API changes

**Medium Risk:**
- Business logic changes
- Data access changes
- Configuration changes
- Installer changes

**Low Risk:**
- UI text changes
- Logging changes
- Code formatting
- Documentation only

---

## Integration with TodoWrite

When reviewing changes, use TodoWrite to track review tasks:

```javascript
TodoWrite([
  {content: "Review security implications", status: "in_progress", activeForm: "Reviewing security"},
  {content: "Search for similar patterns", status: "pending", activeForm: "Searching for patterns"},
  {content: "Verify test coverage", status: "pending", activeForm: "Verifying tests"},
  {content: "Generate review summary", status: "pending", activeForm: "Generating summary"}
])
```

---

## Best Practices

1. **Be Thorough:** Don't skip steps, especially the "missed changes" search
2. **Be Specific:** Point to exact file and line numbers
3. **Be Constructive:** Provide solutions, not just problems
4. **Be Consistent:** Apply same standards to all reviews
5. **Be Security-Conscious:** Always check for security implications
6. **Be Complete:** Check runtime, installer, tools, and tests
7. **Be Practical:** Prioritize issues by severity and impact
8. **Document the Review:** ALWAYS include "Change Review Summary" in feature documentation with complete review details, issues found/fixed, and merge readiness assessment

---

## Output Format

Always provide:
1. **Executive Summary** - High-level overview
2. **Detailed Findings** - Specific issues with locations
3. **Recommendations** - Actionable next steps
4. **Checklist** - Items to verify before merge

Use visual indicators:
- ✓ = Good/Complete
- ⚠️ = Warning/Needs attention
- ❌ = Critical issue/Must fix
- 💡 = Suggestion/Enhancement

---

## Notes

- This protocol assumes access to Azure DevOps MCP tools
- Adjust search patterns based on project structure
- Some steps may not apply to all changes (e.g., installer changes for documentation updates)
- Use judgment to skip irrelevant sections
- Always ask for clarification when uncertain
