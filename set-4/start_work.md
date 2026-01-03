# Start Work Protocol

## Trigger Phrases
- "start work on {work item number}"
- "begin work on {work item number}"
- "start {work item number}"
- "work on {work item number}"

## Overview

This protocol helps you start work on a new feature or bug fix by:
1. Fetching work item details from Azure DevOps
2. Creating an appropriate branch from the latest code
3. Analyzing requirements and locating impacted code
4. Creating initial documentation
5. Suggesting initial changes

## Protocol Steps

### 1. Extract Work Item Number

**Actions:**
- [ ] Parse user input to extract work item number
- [ ] Validate work item number is numeric

**Pattern Matching:**
```
"start work on 115163" → 115163
"begin work on #115163" → 115163
"work on 115163" → 115163
```

**If Invalid:**
```
I need a valid work item number to start work.

Example: "start work on 115163"
```

---

### 2. Fetch Work Item Details

**Actions:**
- [ ] Fetch work item from Azure DevOps
- [ ] Review title and description
- [ ] Check acceptance criteria
- [ ] Review related work items
- [ ] Check comments for additional context
- [ ] Identify work item type (Feature, Bug, Task, etc.)

**Azure DevOps Queries:**
```javascript
mcp__azure-devops__wit_get_work_item(
  id: {workItemNumber},
  project: "CadLink",
  expand: "relations"
)
mcp__azure-devops__wit_list_work_item_comments(
  project: "CadLink",
  workItemId: {workItemNumber}
)
```

**Extract:**
- Work item title
- Work item type (Feature, Bug, Task, User Story, etc.)
- Description and acceptance criteria
- Related work items (parent, children, dependencies)
- Priority and severity
- Assigned to
- Tags
- Comments with implementation guidance

**If Work Item Not Found:**
```
⚠️ Work item #{number} not found in Azure DevOps.

Please verify:
1. The work item number is correct
2. The work item exists in the CadLink project
3. You have access to view this work item
```

**Output:** Display work item summary
```
📋 WORK ITEM #{number}: {title}

Type: {Feature|Bug|Task}
Priority: {priority}
State: {state}
Assigned To: {name}

Description:
{description}

Acceptance Criteria:
{criteria}

Related Work Items:
- Parent: #{number} - {title}
- Related: #{number} - {title}

Recent Comments:
{recent comments with implementation guidance}
```

---

### 3. Determine Base Branch

**Actions:**
- [ ] Check if repository uses `master` or `main`
- [ ] Fetch latest changes from base branch
- [ ] Verify base branch is up to date

**Commands:**
```bash
# Check which base branch exists
git branch -r | grep -E "origin/(master|main)$"

# Fetch latest changes
git fetch origin

# Get latest commit on base branch
git log origin/master -1 --oneline  # or origin/main
```

**Output:**
```
🌿 Base Branch: {master|main}
Latest Commit: {hash} - {message}
Date: {date}
```

---

### 4. Determine Branch Name

**Actions:**
- [ ] Determine branch prefix based on work item type
- [ ] Create descriptive branch name from work item title
- [ ] Validate branch name follows conventions

**Branch Naming Rules:**
- **Feature** → `features/{workitem}-{description}`
- **Bug** → `bugs/{workitem}-{description}`
- **Task** → `tasks/{workitem}-{description}`
- **User Story** → `features/{workitem}-{description}`

**Description Generation:**
- Take work item title
- Convert to kebab-case
- Limit to 3-5 key words
- Remove articles (a, an, the)
- Remove special characters

**Examples:**
```
Work Item #115163: "Fix plaintext password security vulnerability"
→ bugs/115163-plaintext-password-fix

Work Item #120000: "Add batch size auto-scaling feature"
→ features/120000-batch-size-autoscaling

Work Item #125000: "Refactor database connection pooling"
→ tasks/125000-database-connection-pooling
```

**Output:**
```
📝 Proposed Branch Name: {branch-name}
```

**Ask User for Confirmation:**
```
Is this branch name acceptable, or would you like to use a different name?
```

---

### 5. Create Branch

**Actions:**
- [ ] Ensure working directory is clean
- [ ] Switch to base branch
- [ ] Pull latest changes
- [ ] Create new branch from base
- [ ] Push new branch to remote (optional)

**Commands:**
```bash
# Check for uncommitted changes
git status --short

# If clean, proceed
git checkout {base-branch}
git pull origin {base-branch}
git checkout -b {new-branch-name}

# Optionally push to remote
git push -u origin {new-branch-name}
```

**If Working Directory Not Clean:**
```
⚠️ Working directory has uncommitted changes.

Options:
1. Commit current changes first
2. Stash changes: git stash
3. Discard changes: git reset --hard (CAREFUL!)

What would you like to do?
```

**Output:**
```
✓ Branch created: {branch-name}
✓ Based on: {base-branch} ({commit-hash})
✓ Pushed to remote: {yes|no}
```

---

### 6. Analyze Requirements and Locate Impacted Code

**Actions:**
- [ ] Parse acceptance criteria and description
- [ ] Identify key components mentioned
- [ ] Search for potentially impacted files
- [ ] Identify similar existing features
- [ ] Locate relevant code patterns

**Analysis Questions:**
1. **What components are mentioned?** (Server, Client, Database, Installer, etc.)
2. **What functionality is affected?** (Authentication, Encryption, Data transfer, etc.)
3. **Are there similar features?** (Search for related code)
4. **What patterns should be used?** (Look for existing implementations)
5. **What needs to be modified?** (Existing code vs new code)

**Search Strategy:**

**By Component Name:**
```bash
# Search for component references in work item
grep -r "component_name" --include="*.cs"
```

**By Feature/Functionality:**
```bash
# Search for related functionality
grep -r "encryption\|password\|security" --include="*.cs"
grep -r "batch\|scaling\|performance" --include="*.cs"
```

**By File Patterns:**
```bash
# Find relevant files by name
find . -name "*Encryption*" -type f
find . -name "*Password*" -type f
```

**By Similar Work Items:**
```javascript
// Search for related work items
mcp__azure-devops__search_workitem({
  searchText: "{keywords from title}",
  project: ["CadLink"],
  state: ["Closed", "Resolved"],
  top: 10
})
```

**Output:**
```
🔍 CODE ANALYSIS

Potentially Impacted Components:
1. {Component 1} - {reason}
   Files: {list of files}

2. {Component 2} - {reason}
   Files: {list of files}

Similar Features:
- Work Item #{number}: {title}
  Files: {files}

Relevant Code Patterns:
- {Pattern 1}: {files using this pattern}
- {Pattern 2}: {files using this pattern}

Recommended Starting Points:
1. {file path} - {reason}
2. {file path} - {reason}
```

---

### 7. Create Initial Feature Documentation

**Actions:**
- [ ] Call "create documentation" protocol
- [ ] Pass work item details and analysis results
- [ ] Ensure documentation created successfully

**Protocol to Call:**
Use the `create_documentation.md` protocol which will:
- Create documentation file in `docs/notes/`
- Use appropriate naming convention
- Include all required sections
- Mark sections as TODO where details aren't known yet
- Set up placeholder for Change Review Summary
- Set up placeholder for Test Plan reference

**Note:** This step delegates to the "create documentation" protocol.

**What "create documentation" protocol does:**
- Creates comprehensive documentation with all required sections
- Includes work item details
- Sets up placeholders for Change Review Summary
- Sets up placeholders for Test Plan reference
- Marks TODOs appropriately

**Output:**
```
📄 DOCUMENTATION CREATED VIA PROTOCOL

File: docs/notes/{filename}.md
Created By: "create documentation" protocol

Documentation includes:
✓ Work item details and acceptance criteria
✓ Problem statement (TODO)
✓ Solution approach (TODO)
✓ Potentially impacted code from analysis
✓ Implementation plan (TODO)
✓ Testing strategy (TODO)
✓ Placeholder for Change Review Summary
✓ Placeholder for Test Plan reference
✓ Questions and decisions tracking
✓ All standard documentation sections

Status: Ready for implementation
Next: Fill in TODOs as you implement
```

---

### 8. Create Initial Test Plan

**Actions:**
- [ ] Create test plan file in `docs/testing/`
- [ ] Use matching naming convention to feature docs
- [ ] Include initial test categories
- [ ] Link to feature documentation

**Test Plan File Naming:**
```
Match the feature documentation name:
docs/notes/KeyDatabaseLegacy-PasswordSecurityFix.md
→ docs/testing/KeyDatabaseLegacy-PasswordSecurityFix.md

docs/notes/115163-PlaintextPasswordFix.md
→ docs/testing/115163-PlaintextPasswordFix.md
```

**Initial Test Plan Template:**

```markdown
# Test Plan: {Work Item Title} (Work Item #{number})

## Overview

**Feature:** {title}
**Work Item:** #{work-item-id}
**Branch:** `{branch-name}`
**Status:** 🚧 Draft

**Feature Documentation:** [Link](../notes/{matching-filename}.md)

## Test Objectives

Based on the work item acceptance criteria, this test plan validates:

{List objectives from acceptance criteria}

**TODO:** Refine objectives as implementation progresses

## Scope

### In Scope
{What will be tested}

**TODO:** Update as implementation details are finalized

### Out of Scope
{What will NOT be tested}

## Test Strategy

### Test Levels

**Unit Tests**
- Component: {component}
- Coverage: {what to cover}

**Integration Tests**
- Components: {components}
- Interactions: {what interactions to test}

**System Tests**
- End-to-end scenarios: {scenarios}

**Manual Tests**
- User acceptance: {scenarios}
- Exploratory testing: {areas}

**TODO:** Expand strategy based on implementation

## Test Environment

**Requirements:**
- Operating System: {OS}
- Database: {DB}
- Configuration: {special config}
- Test Data: {data requirements}

**TODO:** Define specific environment requirements

## Test Scenarios

### Scenario 1: {Scenario Name}

**Objective:** {What this scenario tests}

**Preconditions:**
- {Precondition 1}
- {Precondition 2}

**Test Cases:**

#### TC-1.1: {Test Case Name}
- **Priority:** {High|Medium|Low}
- **Type:** {Functional|Security|Performance|Usability}
- **Steps:**
  1. {Step 1}
  2. {Step 2}
  3. {Step 3}
- **Expected Result:** {expected outcome}
- **Status:** ⏳ Not Started

**TODO:** Add more scenarios based on acceptance criteria

## Automated Test Coverage

### Unit Tests

**File:** `Tests/CadLink.Tests/{TestClassName}.cs`

**Tests to Create:**
1. `Test_{MethodName}_{Scenario}_Returns{Expected}()`
2. `Test_{MethodName}_{ErrorCase}_Throws{Exception}()`

**TODO:** Implement as code is written

### Integration Tests

**File:** `Tests/CadLinkAutomatedTests/{TestClassName}.cs`

**Tests to Create:**
1. {Test description}

**TODO:** Implement as components are integrated

## Manual Test Procedures

### Procedure 1: {Procedure Name}

**Purpose:** {What this validates}

**Steps:**
1. {Step 1}
2. {Step 2}
3. {Step 3}

**Verification:**
- [ ] {Check 1}
- [ ] {Check 2}

**TODO:** Expand manual test procedures

## Regression Testing

**Areas to Verify:**
- {Existing functionality that might be affected}

**Regression Test Suite:**
- [ ] Run existing unit tests
- [ ] Run existing integration tests
- [ ] Manual smoke test of {features}

**TODO:** Identify specific regression tests

## Security Testing

**Security Test Cases:**
1. {Security test 1}
2. {Security test 2}

**Security Verification:**
- [ ] {Security check 1}
- [ ] {Security check 2}

**TODO:** Define security tests if applicable

## Performance Testing

**Performance Benchmarks:**
- {Metric 1}: {current baseline} → {target}
- {Metric 2}: {current baseline} → {target}

**Load Testing:**
- {Scenario}: {load profile}

**TODO:** Define performance tests if applicable

## Test Data

**Required Test Data:**
1. {Data type 1} - {description}
2. {Data type 2} - {description}

**Data Setup:**
{How to create/obtain test data}

**TODO:** Create test data as needed

## Pass/Fail Criteria

**Feature Acceptance:**
- ✓ All acceptance criteria met
- ✓ All unit tests passing
- ✓ All integration tests passing
- ✓ No critical or high-severity bugs
- ✓ Manual test procedures completed successfully
- ✓ Security validation passed
- ✓ Performance benchmarks met
- ✓ Regression tests passed

## Test Schedule

**TODO:** Define test timeline

- **Unit Testing:** {timeline}
- **Integration Testing:** {timeline}
- **System Testing:** {timeline}
- **UAT:** {timeline}

## Risks and Mitigations

**Testing Risks:**
1. **Risk:** {risk description}
   **Impact:** {High|Medium|Low}
   **Mitigation:** {mitigation strategy}

**TODO:** Identify testing risks

## Test Deliverables

- [ ] Unit test code
- [ ] Integration test code
- [ ] Manual test procedures
- [ ] Test results documentation
- [ ] Bug reports (if any)
- [ ] Test summary report

## Approval

**TODO:** Complete after testing

- [ ] All tests executed
- [ ] All critical bugs resolved
- [ ] Test results reviewed
- [ ] Feature accepted by {stakeholder}

---

**Last Updated:** {date}
**Status:** 🚧 Draft
**Next Steps:** {what to do next}
```

**Output:**
```
📋 Created: docs/testing/{filename}.md

Initial test plan includes:
✓ Test objectives from acceptance criteria
✓ Test strategy outline
✓ Test scenario templates
✓ Automated test placeholders
✓ Manual test procedure templates
✓ Pass/fail criteria
```

---

### 9. Suggest Initial Changes

**Actions:**
- [ ] Based on work item and code analysis, suggest initial implementation approach
- [ ] Identify files to modify
- [ ] Suggest code patterns to follow
- [ ] Recommend similar implementations to reference

**Suggestion Format:**

```markdown
💡 SUGGESTED IMPLEMENTATION APPROACH

Based on the work item requirements and code analysis:

## Recommended Starting Point

1. **Start with:** {file path}:{line number}
   **Reason:** {why this is a good starting point}
   **Similar code:** {reference to similar implementation}

## Implementation Steps

### Step 1: {Step Description}
**Files to modify:**
- `{file path}` - {what to change}

**Pattern to follow:**
Look at `{reference file}` for similar implementation.

**Suggested changes:**
```csharp
// Current code (if modifying existing):
{current code}

// Suggested change:
{suggested code}
```

### Step 2: {Step Description}
{Continue with more steps}

## Code Patterns to Use

Based on similar features in the codebase:

**Pattern 1: {Pattern Name}**
- Used in: {files}
- Apply to: {your implementation}
- Example:
```csharp
{code example}
```

## Tests to Implement

### Unit Tests
File: `Tests/CadLink.Tests/{TestClass}.cs`
```csharp
[Test]
public void Test_{YourMethod}_{Scenario}_Returns{Expected}()
{
    // Arrange
    {setup code}

    // Act
    {action code}

    // Assert
    {assertion code}
}
```

## Next Steps

1. [ ] Review suggested implementation approach
2. [ ] Modify {file} to implement {feature}
3. [ ] Create unit tests
4. [ ] Update documentation with actual changes
5. [ ] Run tests and verify
6. [ ] Create pull request when ready
```

**Important Notes:**
- Suggestions are starting points, not requirements
- User should review and adapt based on specific needs
- Code examples should follow existing patterns in codebase
- Always include test creation in suggestions

---

### 10. Provide Work Summary and Next Steps

**Actions:**
- [ ] Summarize all work done
- [ ] List created files
- [ ] Provide clear next steps
- [ ] Offer to help with first implementation step

**Summary Template:**

```markdown
✅ WORK STARTED ON #{work-item-id}

## Summary

**Work Item:** #{number} - {title}
**Branch:** `{branch-name}` (created from {base-branch})
**Type:** {Feature|Bug|Task}
**Priority:** {priority}

## Files Created

📄 **Feature Documentation**
- `docs/notes/{filename}.md`
  - Work item details
  - Problem statement (TODO)
  - Potentially impacted code identified
  - Implementation plan (TODO)

📋 **Test Plan**
- `docs/testing/{filename}.md`
  - Test objectives from acceptance criteria
  - Test strategy outline
  - Test scenario templates

## Code Analysis Results

**Potentially Impacted Components:**
{list}

**Files to Review:**
{list}

**Similar Implementations:**
{list}

## Next Steps

1. **Review the documentation:**
   - `docs/notes/{filename}.md`
   - `docs/testing/{filename}.md`

2. **Review the suggested implementation approach above**

3. **Start implementation:**
   - Modify identified files
   - Follow similar patterns in codebase
   - Write tests alongside code

4. **Update documentation as you work:**
   - Remove TODOs
   - Document actual changes
   - Update test plan with specific test cases

5. **When ready for review:**
   - Say "review my changes" to run the code review protocol
   - The review will verify completeness across all components

## How Can I Help?

I can help you with:
- "Show me {file path}" - Display relevant code
- "Search for {pattern}" - Find similar code
- "Help me implement {feature}" - Provide detailed guidance
- "Create tests for {class}" - Generate test templates
- "Review my changes" - Run comprehensive code review (when ready)

What would you like to do next?
```

---

## Error Handling

### Work Item Not Found
```
❌ Work item #{number} not found.

Please check:
1. Work item number is correct
2. Work item exists in CadLink project
3. You have access to the work item

Try: mcp__azure-devops__search_workitem to search for the work item
```

### Branch Already Exists
```
⚠️ Branch '{branch-name}' already exists.

Options:
1. Switch to existing branch: git checkout {branch-name}
2. Use different branch name: start work on {number} (I'll suggest alternative)
3. Delete existing branch: git branch -D {branch-name} (CAREFUL!)

What would you like to do?
```

### Cannot Determine Base Branch
```
❌ Could not determine base branch (master or main).

Please specify: "start work on {number} from {master|main}"
```

### Uncommitted Changes
```
⚠️ You have uncommitted changes in your working directory.

Options:
1. Commit changes first
2. Stash changes: git stash
3. Continue anyway (not recommended)

What would you like to do?
```

---

## Example Usage

### Example 1: Start Work on Bug Fix

**User:** "start work on 115163"

**Protocol:**
1. ✓ Fetch work item #115163 from Azure DevOps
2. ✓ Display work item summary (Bug - Plaintext password security)
3. ✓ Determine base branch: master
4. ✓ Propose branch: `bugs/115163-plaintext-password-fix`
5. ✓ Create branch from origin/master
6. ✓ Search for password/Process.Start patterns
7. ✓ Create `docs/notes/KeyDatabaseLegacy-PasswordSecurityFix.md`
8. ✓ Create `docs/testing/KeyDatabaseLegacy-PasswordSecurityFix.md`
9. ✓ Suggest starting with LegacySupport.cs, CustomAction.cs
10. ✓ Provide next steps

### Example 2: Start Work on Feature

**User:** "begin work on 120000"

**Protocol:**
1. ✓ Fetch work item #120000 (Feature - Batch size auto-scaling)
2. ✓ Display work item with acceptance criteria
3. ✓ Determine base branch: main
4. ✓ Propose branch: `features/120000-batch-size-autoscaling`
5. ✓ Create branch
6. ✓ Search for batch processing code
7. ✓ Create documentation and test plan
8. ✓ Suggest implementation approach
9. ✓ Provide next steps

---

## Integration with Other Protocols

### After Starting Work

When implementation is complete:
1. **Run Code Review:** "review my changes"
   - See `.claude/protocols/change_review.md`
   - Will verify completeness and generate review summary

2. **Create/Update Test Plan:** "create test plan"
   - See `.claude/protocols/create_test_plan.md`
   - Will generate detailed test plan based on changes

### Workflow

```
start work on {number}
    ↓
[Implement feature]
    ↓
review my changes
    ↓
create test plan (if not already detailed)
    ↓
[Run tests]
    ↓
[Create pull request]
```

---

## Customization

### Project-Specific Searches

Add searches specific to CadLink:

```bash
# Encryption-related work
grep -r "SQLiteCrypto\|Encryption\|DatabaseKey" --include="*.cs"

# Installer work
find Source/Tools/Installers -name "*.wxs"

# Configuration work
grep -r "ConfigurationSettings\|CadLinkServerSettings\|CadLinkClientSettings" --include="*.cs"
```

### Work Item Type Mapping

Customize branch prefixes:
```
Bug → bugs/
Feature → features/
Task → tasks/
User Story → features/
Technical Debt → refactor/
```

---

## Notes

- This protocol assumes access to Azure DevOps MCP tools
- Requires Git repository to be initialized
- Base branch (master/main) must exist
- User should have permissions to create branches
- Documentation and test plan are starting points - update as you implement
- Always run code review protocol before creating pull request
