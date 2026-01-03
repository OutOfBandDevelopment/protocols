# Create Test Plan Protocol

## Trigger Phrases
- "create test plan"
- "generate test plan"
- "create testing documentation"
- "generate tests for this change"

## Overview

This protocol creates comprehensive test plans based on completed code reviews. It requires that the change review protocol has been run first to ensure all changes are documented and understood.

**Prerequisites:**
- Change review protocol completed (`.claude/protocols/change_review.md`)
- Feature documentation exists in `docs/notes/` with Change Review Summary
- Changes are committed or well-documented

## Protocol Steps

### 1. Verify Change Review Completed

**Actions:**
- [ ] Check current git branch
- [ ] Search for feature documentation in `docs/notes/`
- [ ] Verify Change Review Summary exists in documentation
- [ ] Extract branch name and work item number

**Commands:**
```bash
# Check current branch
git branch --show-current

# Search for feature documentation
ls docs/notes/*.md | grep -v README
```

**Validation:**
Look for documentation file containing:
- "Change Review Summary" section
- "Components Reviewed and Updated" section
- "Overall Assessment" section

**If Change Review Not Found:**
```
⚠️ PREREQUISITE MISSING: Change Review Required

Before creating a test plan, you must run a code review to ensure:
- All changes are documented
- All components are identified
- Security implications are understood
- Completeness is verified

Please run: "review my changes"

After the review is complete, run this protocol again.
```

**Output:**
```
✓ Change review found
✓ Branch: {branch-name}
✓ Work Item: #{number}
✓ Feature Documentation: docs/notes/{filename}.md
```

---

### 2. Extract Information from Change Review

**Actions:**
- [ ] Read feature documentation
- [ ] Extract Change Review Summary details
- [ ] Identify all changed components
- [ ] Extract security implications
- [ ] Extract performance implications
- [ ] Identify all changed files
- [ ] Review acceptance criteria

**Information to Extract:**

**From Work Item:**
- Title and description
- Acceptance criteria
- Priority and severity
- Type (Feature, Bug, Security Fix, etc.)

**From Change Review Summary:**
- Components reviewed and updated
- Security analysis (if applicable)
- Issues found and fixed
- Breaking changes
- Build configuration changes

**From Changes Made Section:**
- All modified files
- Before/after code examples
- Environment variables or configuration changes
- Database changes
- API changes

**Output:**
```
📊 EXTRACTED INFORMATION

Work Item: #{number} - {title}
Type: {Feature|Bug|Security Fix}
Priority: {priority}

Components Changed:
- {Component 1}: {files}
- {Component 2}: {files}

Security Implications:
{security details from review}

Performance Implications:
{performance details from review}

Files Modified: {count}
{list of files}

Acceptance Criteria:
{criteria from work item}
```

---

### 3. Determine Test Plan Scope

**Actions:**
- [ ] Identify required test levels
- [ ] Determine test types needed
- [ ] Identify test environments required
- [ ] Determine test data needs

**Test Level Decision Matrix:**

**Unit Tests:** Always required
- Test individual methods and classes
- Test error handling
- Test edge cases

**Integration Tests:** Required if multiple components interact
- Test component interactions
- Test data flow between components
- Test configuration integration

**System Tests:** Required for features, optional for small bug fixes
- End-to-end workflows
- User scenarios
- Installation/upgrade scenarios

**Manual Tests:** Required for UI changes, security fixes, installer changes
- User acceptance testing
- Security validation
- Upgrade testing

**Performance Tests:** Required if performance implications noted
- Load testing
- Stress testing
- Benchmark comparisons

**Security Tests:** Required for security fixes or changes to authentication/authorization/encryption
- Vulnerability validation
- Attack vector testing
- Penetration testing scenarios

**Output:**
```
📋 TEST SCOPE

Test Levels Required:
✓ Unit Tests
✓ Integration Tests
✓ System Tests
✓ Manual Tests
{✓|—} Performance Tests
{✓|—} Security Tests

Test Types:
- Functional: {scenarios}
- Security: {scenarios}
- Performance: {scenarios}
- Usability: {scenarios}
- Regression: {areas}
```

---

### 3.1 Analyze End-to-End Application Flow

**Purpose:**
Review the application end-to-end to identify all points where the feature needs to be tested, not just the changed code.

**Actions:**
- [ ] Trace feature through entire application flow
- [ ] Identify all entry points (UI, API, CLI, installers, etc.)
- [ ] Identify all data flow paths
- [ ] Identify all components that interact with changed code
- [ ] Identify all user workflows that use this feature
- [ ] Find upstream and downstream dependencies

**Application Flow Analysis:**

**1. Identify Entry Points**

Where can users/systems access this feature?

```bash
# Search for UI entry points
grep -r "{feature keyword}" --include="*.xaml" --include="*.cs" Source/CadLink.Client/
grep -r "{feature keyword}" --include="*.xaml" --include="*.cs" Source/CadLink.Server/

# Search for API entry points
grep -r "public.*{MethodName}" --include="*.cs" Source/

# Search for command-line entry points
grep -r "CommandLine\|args\[" --include="*.cs" InternalTools/

# Search for installer entry points
grep -r "CustomAction" --include="*.wxs" --include="*.cs" Source/Tools/
```

**Entry Points to Test:**
- **UI:** {buttons, menus, dialogs, screens}
- **API:** {endpoints, methods, services}
- **CLI:** {commands, arguments, scripts}
- **Installers:** {install, upgrade, uninstall}
- **Configuration:** {settings files, environment variables}

**2. Trace Data Flow**

Follow data through the application:

```
User Input → Validation → Processing → Storage → Retrieval → Display
     ↓           ↓            ↓          ↓          ↓          ↓
   Test?       Test?        Test?     Test?      Test?      Test?
```

**Data Flow Points:**
1. **Input:** Where does data enter? (UI forms, API calls, config files)
2. **Validation:** Where is data validated? (Client, Server, Database)
3. **Processing:** Where is data transformed? (Business logic)
4. **Storage:** Where is data persisted? (Database, files, cache)
5. **Retrieval:** Where is data read from? (Queries, file reads)
6. **Display:** Where is data presented? (UI, reports, logs)

**Test Each Point:**
- Input validation (valid, invalid, edge cases)
- Processing logic (success, failure, edge cases)
- Storage integrity (write, read, update, delete)
- Retrieval accuracy (correct data returned)
- Display correctness (formatted properly)

**3. Identify Component Interactions**

Find all components that interact with changed code:

```bash
# Find references to changed classes
grep -r "ClassName" --include="*.cs"

# Find references to changed methods
grep -r "MethodName" --include="*.cs"

# Find configuration dependencies
grep -r "{ConfigKey}" --include="*.config" --include="*.xml"

# Find database dependencies
grep -r "{TableName}\|{ColumnName}" --include="*.cs"
```

**Component Interaction Map:**
```
Changed Component: {Name}
    ↓ Called by:
    ├─ Component A (Server)
    ├─ Component B (Client)
    └─ Component C (Installer)
    ↓ Calls:
    ├─ Component D (Database)
    ├─ Component E (Logging)
    └─ Component F (External Service)
```

**Test Interactions:**
- Component A → Changed Component (integration test)
- Changed Component → Component D (integration test)
- Full flow: A → Changed → D (system test)

**4. Trace User Workflows**

Identify complete user workflows that use this feature:

**Example: Database Encryption Feature**
```
Workflow 1: New Installation
User → Run Installer → Installer reads config → Calls DbKeyCustomAction
     → DbKeyCustomAction encrypts database → User starts application
     → Application reads encrypted database
TEST: Install → Verify encryption → Start app → Verify data access

Workflow 2: Upgrade from Old Version
User → Run Installer → Installer detects old version → Backs up database
     → DecryptDatabases (with fix) → Encrypts with new method → Complete
TEST: Upgrade → Verify migration → Verify data integrity

Workflow 3: Manual Re-keying
User → Opens EncryptionManager → Selects databases → Enters passwords
     → Tool calls KeyDatabaseLegacy.exe (with fix) → Databases re-keyed
TEST: Use tool → Re-key → Verify new key works

Workflow 4: Runtime Version Check
Application starts → Checks database version → Calls LegacySupport.GetVersion
     → Returns version → Application proceeds
TEST: App start → Version check → Correct version → No errors
```

**Workflow Testing Strategy:**
- Test each workflow end-to-end
- Test workflow variations (success, failure, edge cases)
- Test workflow transitions (one workflow to another)
- Test workflow cancellation/interruption

**5. Find Upstream Dependencies**

What triggers/enables this feature?

```bash
# Find what calls the changed code
grep -r "\.{MethodName}\(" --include="*.cs"

# Find configuration that enables feature
grep -r "{FeatureName}.*Enable\|{FeatureName}.*Flag" --include="*.config" --include="*.xml"

# Find prerequisites
grep -r "if.*{Condition}" --include="*.cs" {changed-files}
```

**Upstream Test Points:**
- **Prerequisites:** Test with prerequisites met/not met
- **Triggers:** Test all ways feature can be triggered
- **Enabling Conditions:** Test feature enabled/disabled

**6. Find Downstream Dependencies**

What depends on this feature working?

```bash
# Find code that depends on results
grep -r "{ResultVariable}\|{ReturnValue}" --include="*.cs"

# Find features that build on this one
grep -r "{FeatureName}" --include="*.cs" | grep -v "{changed-files}"
```

**Downstream Test Points:**
- **Dependent Features:** Test they still work with changes
- **Side Effects:** Test for unintended consequences
- **Error Propagation:** Test error handling in dependent code

**7. Identify All Test Points**

Comprehensive list of where testing is needed:

**Direct Test Points** (from changed code):
- {Component 1}: {specific functionality}
- {Component 2}: {specific functionality}

**Indirect Test Points** (from flow analysis):
- **Entry Points:** {UI/API/CLI/Installer entry points}
- **Data Flow:** {Input → Validation → Processing → Storage → Retrieval → Display}
- **Component Interactions:** {Calling components and called components}
- **User Workflows:** {End-to-end workflows}
- **Upstream:** {Triggers, prerequisites, enabling conditions}
- **Downstream:** {Dependent features, side effects}

**Integration Points:**
- {Component A} ↔ {Changed Component}
- {Changed Component} ↔ {Component B}
- {External System} ↔ {Changed Component}

**Configuration Points:**
- {Config file} → {Changed Component}
- {Environment Variable} → {Changed Component}
- {Database Setting} → {Changed Component}

**Output:**
```
🔍 END-TO-END ANALYSIS COMPLETE

Application Flow Coverage:
✓ {count} entry points identified
✓ {count} data flow stages mapped
✓ {count} component interactions found
✓ {count} user workflows traced
✓ {count} upstream dependencies identified
✓ {count} downstream dependencies identified

Test Points Identified:
- Direct (changed code): {count}
- Indirect (flow analysis): {count}
- Integration points: {count}
- Configuration points: {count}
Total: {count} test points

These test points will be used to create comprehensive test scenarios.
```

---

### 4. Identify Test Scenarios from Changes and Application Flow

**Actions:**
- [ ] For each changed component, identify test scenarios
- [ ] Map acceptance criteria to test scenarios
- [ ] Identify positive (happy path) scenarios
- [ ] Identify negative (error) scenarios
- [ ] Identify edge cases
- [ ] Identify regression scenarios

**Scenario Identification by Change Type:**

**New Feature:**
- Test all acceptance criteria
- Test feature enabled/disabled
- Test with various inputs
- Test error handling
- Test performance under load

**Bug Fix:**
- Test bug is fixed (reproducer no longer fails)
- Test fix doesn't break existing functionality (regression)
- Test edge cases around the bug
- Test error messages are appropriate

**Security Fix:**
- Test vulnerability is mitigated
- Test attack vectors are blocked
- Test legitimate use still works
- Test security validation tools

**Refactoring:**
- Test behavior unchanged (regression)
- Test performance improved (if applicable)
- Test no new bugs introduced

**Configuration Change:**
- Test with new configuration values
- Test with missing configuration (defaults)
- Test with invalid configuration (error handling)
- Test upgrade from old configuration

**Database Change:**
- Test with empty database
- Test with existing data
- Test upgrade/migration
- Test rollback
- Test data integrity

**Output:**
```
📝 TEST SCENARIOS IDENTIFIED

Scenario 1: {Scenario Name}
- Based on: {Acceptance Criteria #1 / Bug Fix / Security Fix}
- Type: {Functional|Security|Performance}
- Priority: {High|Medium|Low}
- Components: {list}

Scenario 2: {Scenario Name}
- Based on: {source}
- Type: {type}
- Priority: {priority}
- Components: {list}

{Continue for all scenarios}
```

---

### 5. Create Detailed Test Cases

**Actions:**
- [ ] For each scenario, create specific test cases
- [ ] Include preconditions, steps, and expected results
- [ ] Assign priority to each test case
- [ ] Map test cases to requirements
- [ ] Identify automation candidates

**Test Case Template:**

```markdown
#### TC-{Scenario}.{Number}: {Test Case Name}

**Priority:** {Critical|High|Medium|Low}
**Type:** {Functional|Security|Performance|Usability|Regression}
**Automation:** {Automated|Manual|Candidate}
**Component:** {component name}

**Objective:**
{What this test case validates}

**Preconditions:**
- {Precondition 1}
- {Precondition 2}

**Test Data:**
{Specific test data needed}

**Steps:**
1. {Step 1 with specific details}
2. {Step 2 with specific details}
3. {Step 3 with specific details}

**Expected Result:**
{Detailed expected outcome}

**Actual Result:**
{To be filled during testing}

**Status:** ⏳ Not Started

**Related Code:**
- {file path}:{line number}

**Related Requirement:**
- Acceptance Criteria: {which one}
```

**Test Case Numbering:**
- TC-1.1, TC-1.2, TC-1.3 (Scenario 1 test cases)
- TC-2.1, TC-2.2 (Scenario 2 test cases)

---

### 6. Define Automated Tests

**Actions:**
- [ ] Identify which test cases can be automated
- [ ] Create unit test specifications
- [ ] Create integration test specifications
- [ ] Provide test code templates

**For Each Changed Class/Method:**

**Unit Test Specification:**

```markdown
### Unit Tests for {ClassName}

**File:** `Tests/CadLink.Tests/{ClassName}Tests.cs`

#### Test: {MethodName}_With{ValidInput}_Returns{ExpectedResult}

**Purpose:** Verify {method} returns correct result with valid input

**Test Code Template:**
```csharp
[Test]
public void {MethodName}_With{ValidInput}_Returns{ExpectedResult}()
{
    // Arrange
    var sut = new {ClassName}();
    var input = {test input};
    var expected = {expected result};

    // Act
    var actual = sut.{MethodName}(input);

    // Assert
    Assert.AreEqual(expected, actual);
}
```

**Status:** ⏳ To Implement
**Maps to:** TC-{number}

{Continue for all methods}
```

**Integration Test Specification:**

```markdown
### Integration Tests for {ComponentInteraction}

**File:** `Tests/CadLinkAutomatedTests/{Feature}IntegrationTests.cs`

#### Test: {Feature}_EndToEnd_{Scenario}_Succeeds

**Purpose:** Verify end-to-end {feature} workflow

**Test Code Template:**
```csharp
[Test]
public void {Feature}_EndToEnd_{Scenario}_Succeeds()
{
    // Arrange
    {setup multiple components}

    // Act
    {execute workflow}

    // Assert
    {verify end state}
}
```

**Status:** ⏳ To Implement
**Maps to:** TC-{number}
```

---

### 7. Define Manual Test Procedures

**Actions:**
- [ ] Create detailed manual test procedures
- [ ] Include screenshots or UI references where applicable
- [ ] Define verification checkpoints
- [ ] Create step-by-step instructions

**Manual Test Procedure Template:**

```markdown
### Manual Test Procedure {Number}: {Procedure Name}

**Purpose:** {What this procedure validates}

**Required Access:** {Admin|User|Developer}
**Estimated Time:** {minutes}
**Prerequisites:**
- {Prerequisite 1}
- {Prerequisite 2}

**Test Environment Setup:**
1. {Setup step 1}
2. {Setup step 2}

**Procedure Steps:**

**Step 1: {Step Name}**
- Action: {Detailed action to perform}
- Expected: {What should happen}
- Verification: {How to verify}
  - [ ] {Checkpoint 1}
  - [ ] {Checkpoint 2}

**Step 2: {Step Name}**
- Action: {action}
- Expected: {expected}
- Verification:
  - [ ] {checkpoint}

**Step {N}: Verify Results**
- Open {Tool/UI}
- Check: {what to check}
- Expected: {expected state}

**Cleanup:**
1. {Cleanup step 1}
2. {Cleanup step 2}

**Pass Criteria:**
- [ ] All verification checkpoints passed
- [ ] No errors or warnings
- [ ] {Other criteria}

**Notes:**
{Any special notes or known issues}

**Maps to Test Cases:** TC-{number}, TC-{number}
```

---

### 8. Define Security Test Cases (if applicable)

**Actions:**
- [ ] Extract security concerns from change review
- [ ] Create security validation procedures
- [ ] Define attack scenarios to test
- [ ] Create verification commands

**Security Test Template:**

```markdown
## Security Testing

### Security Concern: {Concern from Change Review}

**Risk Before Fix:** {severity}
**Risk After Fix:** {severity}
**Attack Surface:** {what could be exploited}

### Security Test Cases

#### ST-1: Verify {Vulnerability} is Mitigated

**Attack Vector:** {how the attack would work}

**Test Procedure:**
1. {Attempt to exploit vulnerability}
2. {Verify attack is blocked}
3. {Verify proper error handling}

**Expected Security Behavior:**
{What should happen to prevent attack}

**Validation Commands:**
```bash
# {Command to verify security}
{actual command}

# Expected output:
{expected safe output}
```

**Pass Criteria:**
- [ ] Attack is blocked
- [ ] No sensitive data exposed
- [ ] Appropriate error message shown
- [ ] Security logs recorded

#### ST-2: Verify Legitimate Use Still Works

**Purpose:** Ensure security fix doesn't break normal functionality

**Test Procedure:**
1. {Perform legitimate operation}
2. {Verify it works correctly}
3. {Verify no security warnings}

**Pass Criteria:**
- [ ] Normal use case works
- [ ] No false positives
- [ ] Performance acceptable
```

---

### 9. Define Performance Test Cases (if applicable)

**Actions:**
- [ ] Extract performance concerns from change review
- [ ] Define performance benchmarks
- [ ] Create load test scenarios
- [ ] Define acceptance criteria

**Performance Test Template:**

```markdown
## Performance Testing

### Performance Concern: {Concern from Change Review}

**Current Baseline:** {current metrics}
**Target Metrics:** {target metrics}
**Acceptance Criteria:** {min acceptable performance}

### Performance Test Cases

#### PT-1: {Performance Scenario}

**Metric:** {Response Time|Throughput|CPU|Memory}

**Test Configuration:**
- Users: {number}
- Duration: {time}
- Data Volume: {size}
- Environment: {specs}

**Test Procedure:**
1. {Setup monitoring}
2. {Execute load}
3. {Collect metrics}
4. {Analyze results}

**Benchmark Comparison:**
| Metric | Baseline | Target | Actual | Status |
|--------|----------|--------|--------|--------|
| {Metric 1} | {value} | {value} | TBD | ⏳ |
| {Metric 2} | {value} | {value} | TBD | ⏳ |

**Pass Criteria:**
- [ ] Meets or exceeds target metrics
- [ ] No degradation in related areas
- [ ] Stable under sustained load

**Test Tools:**
- {Tool name}: {purpose}
```

---

### 10. Define Regression Test Strategy

**Actions:**
- [ ] Identify areas that might be affected by changes
- [ ] List existing functionality to verify
- [ ] Define regression test suite
- [ ] Identify smoke tests

**Regression Test Template:**

```markdown
## Regression Testing

### Regression Risk Areas

Based on the Change Review Summary, these areas might be affected:

**High Risk:**
- {Area 1}: {why it might be affected}
- {Area 2}: {why it might be affected}

**Medium Risk:**
- {Area 3}: {why it might be affected}

**Low Risk:**
- {Area 4}: {why it might be affected}

### Regression Test Suite

#### Existing Functionality to Verify

**{Component 1}**
- [ ] {Existing feature 1} - Manual test: {procedure}
- [ ] {Existing feature 2} - Automated: `{test class}`

**{Component 2}**
- [ ] {Existing feature 3} - Manual test: {procedure}

### Automated Regression Tests

**Existing Test Suites to Run:**
```bash
# Run all unit tests
dotnet test Tests/CadLink.Tests/

# Run integration tests
dotnet test Tests/CadLinkAutomatedTests/

# Run specific test suite
dotnet test --filter "Category={CategoryName}"
```

**Expected Results:**
- All existing tests should pass
- No new failures introduced

### Smoke Test Checklist

Quick verification that major functionality still works:

**Application Startup:**
- [ ] Server starts without errors
- [ ] Client starts without errors
- [ ] Configuration loads correctly

**Core Functionality:**
- [ ] {Core function 1} works
- [ ] {Core function 2} works
- [ ] {Core function 3} works

**Critical Paths:**
- [ ] {Critical workflow 1} completes
- [ ] {Critical workflow 2} completes
```

---

### 11. Create Complete Test Plan Document

**Actions:**
- [ ] Determine test plan file name (match feature documentation)
- [ ] Generate complete test plan document
- [ ] Include all sections
- [ ] Link to feature documentation
- [ ] Save to `docs/testing/`

**File Naming:**
```
Match the feature documentation:
docs/notes/KeyDatabaseLegacy-PasswordSecurityFix.md
→ docs/testing/KeyDatabaseLegacy-PasswordSecurityFix.md

docs/notes/115163-PlaintextPasswordFix.md
→ docs/testing/115163-PlaintextPasswordFix.md
```

**Complete Test Plan Structure:**

```markdown
# Test Plan: {Feature Title} (Work Item #{number})

## Document Information

**Version:** 1.0
**Created:** {date}
**Last Updated:** {date}
**Author:** {name}
**Status:** {Draft|Review|Approved|In Progress|Completed}

**Related Documentation:**
- Feature Documentation: [Link](../notes/{filename}.md)
- Work Item: [#{number}](https://dev.azure.com/cadwell/CadLink/_workitems/edit/{number})

## Overview

{Brief description of feature and testing objectives}

**Change Summary from Code Review:**
{Extract from Change Review Summary}

## Test Objectives

This test plan validates the following objectives based on the work item acceptance criteria and change review:

1. {Objective 1 from acceptance criteria}
2. {Objective 2 from acceptance criteria}
3. {Security objective if applicable}
4. {Performance objective if applicable}
5. {Regression objective}

## Scope

### In Scope

**Components Under Test:**
{List from Change Review Summary}

**Functionality Covered:**
{List based on changed files}

**Test Types:**
- ✓ Unit Testing
- ✓ Integration Testing
- ✓ System Testing
- ✓ Manual Testing
- {✓|—} Security Testing
- {✓|—} Performance Testing
- ✓ Regression Testing

### Out of Scope

{What is explicitly not being tested}

## Test Strategy

{Generated test strategy based on change type}

## Test Environment

### Environment Requirements

**Operating System:** {from project requirements}
**Database:** {from project requirements}
**.NET Framework:** {from project requirements}
**Special Requirements:** {from change analysis}

### Test Data Requirements

{List of test data needed based on test cases}

### Environment Setup

{Steps to set up test environment}

## Test Scenarios and Cases

{All test scenarios and cases from step 5}

## Automated Test Specifications

{All automated test specs from step 6}

## Manual Test Procedures

{All manual test procedures from step 7}

## Security Testing

{Security tests from step 8 if applicable}

## Performance Testing

{Performance tests from step 9 if applicable}

## Regression Testing

{Regression strategy from step 10}

## Test Schedule

**Unit Testing:** During development
**Integration Testing:** After component integration
**System Testing:** After all changes complete
**Manual Testing:** After automated tests pass
**Regression Testing:** Before merge to main
**UAT:** After all testing complete

**Estimated Timeline:**
- Development & Unit Testing: {estimate}
- Integration Testing: {estimate}
- System & Manual Testing: {estimate}
- Bug Fixes & Retest: {estimate}
- **Total:** {estimate}

## Test Execution Tracking

### Test Case Summary

| ID | Test Case | Priority | Type | Status | Result |
|----|-----------|----------|------|--------|--------|
| TC-1.1 | {name} | {priority} | {type} | ⏳ | — |
| TC-1.2 | {name} | {priority} | {type} | ⏳ | — |

**Status Legend:**
- ⏳ Not Started
- 🔄 In Progress
- ✅ Passed
- ❌ Failed
- ⏸️ Blocked
- ⏭️ Skipped

## Defect Management

### Defect Tracking

| ID | Description | Severity | Status | Test Case |
|----|-------------|----------|--------|-----------|
| — | — | — | — | — |

**Severity Levels:**
- **Critical:** Blocks testing, data loss, security vulnerability
- **High:** Major functionality broken, no workaround
- **Medium:** Functionality impaired, workaround exists
- **Low:** Minor issue, cosmetic

### Defect Resolution Criteria

**Critical/High:** Must be fixed before release
**Medium:** Should be fixed before release
**Low:** Can be deferred to next release

## Pass/Fail Criteria

### Feature Acceptance Criteria

**Must Pass:**
- ✓ All acceptance criteria from work item validated
- ✓ All critical and high priority test cases pass
- ✓ All security tests pass (if applicable)
- ✓ All performance benchmarks met (if applicable)
- ✓ No critical or high severity defects
- ✓ All automated tests pass
- ✓ Regression tests pass

**Should Pass:**
- ✓ All medium priority test cases pass
- ✓ No medium severity defects

**Nice to Have:**
- ✓ All low priority test cases pass
- ✓ No low severity defects

## Risks and Mitigations

### Testing Risks

{Based on change review and analysis}

**Risk 1:** {risk description}
- **Impact:** {High|Medium|Low}
- **Probability:** {High|Medium|Low}
- **Mitigation:** {mitigation strategy}

## Test Deliverables

- [ ] This test plan document
- [ ] Automated test code (unit tests)
- [ ] Automated test code (integration tests)
- [ ] Manual test execution results
- [ ] Defect reports
- [ ] Test summary report
- [ ] Sign-off documentation

## Approval and Sign-Off

### Test Plan Approval

- [ ] **Reviewed by:** {name} — Date: ______
- [ ] **Approved by:** {name} — Date: ______

### Test Execution Sign-Off

- [ ] **All tests executed**
- [ ] **All critical defects resolved**
- [ ] **Test results reviewed**
- [ ] **Feature approved by:** {stakeholder} — Date: ______

---

**Document Version History**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | {date} | {name} | Initial test plan created |

---

**Next Steps:**
1. Review and approve test plan
2. Implement automated tests
3. Execute test cases
4. Log and fix defects
5. Retest
6. Complete sign-off
```

**Output:**
```
✅ TEST PLAN CREATED

File: docs/testing/{filename}.md

Test Plan Includes:
✓ {count} test scenarios
✓ {count} test cases
✓ {count} automated test specifications
✓ {count} manual test procedures
{✓|—} {count} security test cases
{✓|—} {count} performance test cases
✓ Regression test strategy
✓ Pass/fail criteria
✓ Test execution tracking
✓ Defect management process

Test Case Breakdown:
- Critical Priority: {count}
- High Priority: {count}
- Medium Priority: {count}
- Low Priority: {count}

Automation:
- Unit Tests: {count}
- Integration Tests: {count}
- Manual Tests: {count}
```

---

### 12. Update Feature Documentation with Test Plan Reference

**Actions:**
- [ ] Read feature documentation file (same file from step 1)
- [ ] Update "Testing" section with test plan reference
- [ ] Update "Test Plan Status" to "Created"
- [ ] Add link to test plan file
- [ ] Update "Last Updated" date
- [ ] Add version history entry

**Documentation Updates:**

**Find and Update Testing Section:**
```markdown
OLD:
## Testing

**TODO:** Document testing approach

**See:** `docs/testing/{matching-filename}.md` for detailed test plan

**Test Plan Status:** ⏳ Not Created

NEW:
## Testing

**Test Plan:** `docs/testing/{matching-filename}.md` ✅ CREATED

**Test Plan Status:** ✓ Complete

**Test Summary:**
- Test Scenarios: {count}
- Test Cases: {count}
  - Critical: {count}
  - High: {count}
  - Medium: {count}
  - Low: {count}
- Automated Tests: {count} ({percentage}%)
- Manual Tests: {count} ({percentage}%)
- Security Tests: {count}
- Performance Tests: {count}

**See test plan for:**
- Detailed test scenarios and cases
- Automated test specifications
- Manual test procedures
- Security and performance testing
- Regression test strategy
- Test execution tracking
```

**Update Metadata:**
```markdown
# At top of document, update:
**Last Updated:** {current date}

# At bottom of document, add version history entry:
| {version} | {date} | {author} | Test plan created and linked |
```

**Output:**
```
✅ FEATURE DOCUMENTATION UPDATED

File: docs/notes/{filename}.md
Action: Test plan reference added
Updated Sections:
  ✓ Testing section updated with test plan link
  ✓ Test Plan Status changed to "Complete"
  ✓ Test summary statistics added
  ✓ Last Updated date updated
  ✓ Version history entry added

Documentation Status:
  ✓ Feature documentation complete
  ✓ Change Review Summary complete (from "review my changes")
  ✓ Test Plan reference complete
  ✓ Ready for implementation and testing
```

---

### 13. Provide Test Plan Summary

**Actions:**
- [ ] Summarize test plan
- [ ] Provide next steps
- [ ] Offer to help with test implementation

**Summary Template:**

```markdown
✅ COMPREHENSIVE TEST PLAN GENERATED

## Test Plan Summary

**Feature:** #{work-item} - {title}
**Test Plan:** `docs/testing/{filename}.md`
**Feature Documentation:** `docs/notes/{filename}.md`

## Coverage

**Test Scenarios:** {count}
**Test Cases:** {count}
**Automated Tests:** {count} ({percentage}%)
**Manual Tests:** {count} ({percentage}%)

## Test Types

{✓|—} Unit Tests: {count}
{✓|—} Integration Tests: {count}
{✓|—} System Tests: {count}
{✓|—} Manual Tests: {count}
{✓|—} Security Tests: {count}
{✓|—} Performance Tests: {count}
✓ Regression Tests

## Priority Breakdown

- Critical: {count} test cases
- High: {count} test cases
- Medium: {count} test cases
- Low: {count} test cases

## Estimated Testing Effort

**Development:** {time for implementing automated tests}
**Execution:** {time for running all tests}
**Total:** {total estimated time}

## Next Steps

1. **Review the test plan:**
   - Open `docs/testing/{filename}.md`
   - Verify test cases cover all requirements
   - Confirm pass/fail criteria

2. **Review updated feature documentation:**
   - Open `docs/notes/{filename}.md`
   - Verify test plan link is correct
   - Review test summary

3. **Implement automated tests:**
   - Start with unit tests
   - Follow test specifications in plan
   - Run tests as you implement

4. **Execute manual tests:**
   - Follow manual test procedures
   - Document results
   - Log any defects

5. **Track progress:**
   - Update test case status in plan
   - Log defects in defect table
   - Monitor pass/fail criteria

6. **Sign off when complete:**
   - Verify all acceptance criteria met
   - Get stakeholder approval
   - Update test plan with final results

## How Can I Help?

I can help you with:
- "Implement tests for {class}" - Generate test code
- "Create test data for {scenario}" - Generate test data
- "Review test results" - Analyze test outcomes
- "Update test plan" - Modify test plan based on new info

Ready to start testing?
```

---

## Error Handling

### No Change Review Found

```
❌ PREREQUISITE NOT MET

Cannot create test plan without a completed change review.

Required:
1. Feature documentation in docs/notes/
2. Change Review Summary section
3. Documented changes and components

Please run: "review my changes"

Once the review is complete, run this protocol again.
```

### Insufficient Information

```
⚠️ INCOMPLETE CHANGE REVIEW

The change review exists but is missing key information:
{List missing sections}

Please complete the change review first:
- Run "review my changes" again
- Ensure all sections are filled
- Verify Change Review Summary is complete

Then run this protocol again.
```

### Test Plan Already Exists

```
ℹ️ Test plan already exists: docs/testing/{filename}.md

Options:
1. Update existing test plan (recommended)
2. Replace existing test plan
3. Create new test plan with different name

What would you like to do?
```

---

## Integration with Other Protocols

### Workflow

```
start work on {number}
    ↓
[Implement changes]
    ↓
review my changes  ← REQUIRED before create test plan
    ↓
create test plan  ← This protocol
    ↓
[Implement tests]
    ↓
[Execute tests]
    ↓
[Fix defects]
    ↓
[Retest]
    ↓
[Create pull request]
```

---

## Best Practices

1. **Always Review First:** Never create test plan without change review
2. **Be Comprehensive:** Cover all test types (unit, integration, system, manual)
3. **Prioritize:** Focus on critical and high priority tests first
4. **Automate:** Automate wherever possible to improve efficiency
5. **Be Specific:** Provide detailed steps, not vague descriptions
6. **Track Everything:** Use tables to track test execution and defects
7. **Update Regularly:** Keep test plan current as changes are made
8. **Link to Code:** Reference specific files and line numbers
9. **Define Pass/Fail:** Make acceptance criteria clear and measurable
10. **Plan for Regression:** Always include regression testing

---

## Customization

### Project-Specific Test Categories

Add test categories specific to CadLink:

```markdown
### Installer Testing
- Fresh install testing
- Upgrade testing (from specific versions)
- Uninstall testing
- Custom action validation

### Database Migration Testing
- Schema upgrade testing
- Data migration validation
- Rollback testing
- Multi-version upgrade paths

### Encryption Testing
- Legacy encryption compatibility
- SEE encryption validation
- Key rotation testing
- Decryption verification
```

### Test Tools Reference

Include project-specific test tools:

```markdown
### CadLink Test Tools

**Unit Testing:**
- NUnit framework
- Moq for mocking

**Integration Testing:**
- Custom test harness in Tests/CadLinkAutomatedTests/

**Database Testing:**
- SQLite test databases
- Schema comparison tools

**Installer Testing:**
- MSI test environments
- Virtual machine snapshots

**Security Testing:**
- Process monitoring tools
- Network traffic analysis
```

---

## Notes

- This protocol requires change review to be completed first
- Test plan should be updated as implementation progresses
- Automated tests should be implemented alongside feature code
- Manual tests should be executed before merging to main
- Test plan serves as contract for feature acceptance
- All tests must pass before feature is considered complete
