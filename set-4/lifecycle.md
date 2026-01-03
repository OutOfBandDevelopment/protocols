# Development Lifecycle Protocol

## Trigger Phrases
- "show lifecycle"
- "show workflow"
- "what's the development process"
- "development workflow"
- "how do I develop a feature"

## Overview

This protocol documents the complete development lifecycle for CadLink features, from initial work item to pull request. Following this workflow ensures comprehensive documentation, thorough code review, and complete test coverage.

## Complete Development Workflow

```
1. start work on {work item}  ← Create branch and foundation docs
   ↓
2. [Implement changes]         ← Write code, update docs as you go
   ↓
3. review my changes           ← Comprehensive review, adds Change Review Summary
   ↓
4. create test plan            ← Generate test plan, links from docs
   ↓
5. [Implement tests]           ← Write automated and manual tests
   ↓
6. [Execute tests]             ← Run tests, document results
   ↓
7. [Create pull request]       ← Submit for team review
```

---

## Lifecycle Phases

### Phase 1: Start Work

**Trigger:** `start work on {work item number}`

**Protocol:** `.claude/protocols/start_work.md`

**What Happens:**
1. Fetches work item from Azure DevOps
2. Creates feature branch from master
3. Calls "create documentation" protocol
4. Analyzes codebase for similar implementations
5. Suggests implementation approach
6. Provides next steps

**Output:**
- New feature branch: `features/{workitem}-{description}`
- Foundation documentation: `docs/notes/{Component}-{Feature}.md`
- Implementation suggestions based on codebase patterns

**Next Step:** Implement the feature/change

---

### Phase 2: Implement Changes

**Manual Work**

**What To Do:**
1. Write code following implementation plan
2. Update documentation as you work
3. Fill in TODO sections in feature documentation
4. Document decisions and questions
5. Commit changes regularly with descriptive messages

**Tips:**
- Keep documentation current, don't wait until the end
- Mark completed implementation plan steps
- Document any deviations from original plan
- Track questions and get them answered
- Follow security and coding best practices

**Next Step:** Run "review my changes"

---

### Phase 3: Code Review

**Trigger:** `review my changes`

**Protocol:** `.claude/protocols/change_review.md`

**Prerequisites:**
- Feature documentation must exist in `docs/notes/`

**What Happens:**
1. Verifies documentation exists (fails if not found)
2. Analyzes git status and changed files
3. Extracts and validates branch name
4. Retrieves work item from Azure DevOps
5. Reviews all code changes for quality and security
6. Searches entire codebase for missed changes
7. Checks for test results
8. Reviews dependencies and build configuration
9. Updates documentation with Change Review Summary

**Output:**
- Comprehensive Change Review Summary added to feature documentation
- Security vulnerabilities identified
- Missed changes found
- Test status reviewed
- Merge readiness assessment

**Next Step:** Run "create test plan"

---

### Phase 4: Create Test Plan

**Trigger:** `create test plan`

**Protocol:** `.claude/protocols/create_test_plan.md`

**Prerequisites:**
- Change review completed (Change Review Summary must exist)
- Feature documentation must exist in `docs/notes/`

**What Happens:**
1. Verifies change review completed (fails if not found)
2. Extracts change information from Change Review Summary
3. Analyzes end-to-end application flow
4. Creates comprehensive test scenarios and test cases
5. Defines automated test specifications
6. Creates manual test procedures
7. Includes security/performance testing if applicable
8. Defines regression test strategy
9. Generates test plan in `docs/testing/`
10. Updates feature documentation with test plan link

**Output:**
- Comprehensive test plan: `docs/testing/{Component}-{Feature}.md`
- Test scenarios and test cases
- Automated test specifications
- Manual test procedures
- Feature documentation updated with test plan reference

**Next Step:** Implement tests

---

### Phase 5: Implement Tests

**Manual Work**

**What To Do:**
1. Read test plan: `docs/testing/{Component}-{Feature}.md`
2. Implement automated tests (unit, integration)
3. Follow automated test specifications in test plan
4. Add tests to appropriate test projects:
   - Unit tests: `Tests/CadLink.Tests/`
   - Integration tests: `Tests/CadLinkAutomatedTests/`
5. Update test plan with implementation status
6. Mark test cases as implemented

**Test Quality:**
- Follow AAA pattern (Arrange, Act, Assert)
- Use descriptive test names
- Test happy path and error cases
- Mock external dependencies
- Ensure tests are repeatable

**Next Step:** Execute tests

---

### Phase 6: Execute Tests

**Manual Work**

**What To Do:**
1. Run automated tests
2. Execute manual test procedures from test plan
3. Document test results in test plan
4. Mark test cases as:
   - ✅ Passed
   - ❌ Failed
   - 🔄 In Progress
   - ⏳ Not Started
5. Document any defects found
6. Fix defects and retest
7. Update Test Execution Summary in test plan

**Test Execution:**
```bash
# Run unit tests
dotnet test Tests/CadLink.Tests/

# Run integration tests
dotnet test Tests/CadLinkAutomatedTests/
```

**Evidence Collection:**
- Screenshots of manual tests
- Test output logs
- Performance measurements
- Security scan results

**Next Step:** Create pull request (when all tests pass)

---

### Phase 7: Create Pull Request

**Manual Work**

**What To Do:**
1. Ensure all tests pass
2. Review feature documentation is complete
3. Review test plan is complete
4. Commit any final changes
5. Push branch to remote
6. Create pull request in Azure DevOps
7. Link work item to pull request
8. Add reviewers
9. Reference documentation in PR description

**Pull Request Description Template:**
```markdown
## Summary
{Brief description of change}

## Work Item
#{work-item-number} - {title}

## Documentation
- Feature Documentation: `docs/notes/{Component}-{Feature}.md`
- Test Plan: `docs/testing/{Component}-{Feature}.md`
- Change Review: See "Change Review Summary" in feature docs

## Test Results
- Unit Tests: {count} passed
- Integration Tests: {count} passed
- Manual Tests: {count} passed
- All tests: ✅ PASSED

## Checklist
- [x] Documentation complete
- [x] Code review completed
- [x] Test plan created
- [x] All tests passing
- [x] No security vulnerabilities
- [x] No breaking changes (or documented)
```

**Next Step:** Wait for team review and merge

---

## Workflow Variations

### Variation 1: Documentation Already Exists

**Scenario:** Working on existing feature/component that already has documentation

**Workflow:**
```
1. create documentation  ← Update existing or create new
   ↓
2. [Implement changes]
   ↓
3. review my changes  ← Review updates existing docs
   ↓
4. create test plan  ← May update existing test plan
   ↓
... continue as normal
```

### Variation 2: Quick Bug Fix

**Scenario:** Small bug fix that doesn't need full workflow

**Workflow:**
```
1. [Fix bug]
   ↓
2. create documentation  ← Brief documentation
   ↓
3. review my changes
   ↓
4. [Write quick tests]
   ↓
5. [Create PR]
```

**Note:** Still create documentation and run review, but can skip formal test plan for trivial fixes.

### Variation 3: Security Fix

**Scenario:** Security vulnerability requiring comprehensive review

**Workflow:**
```
1. start work on {work item}
   ↓
2. [Implement fix]
   ↓
3. review my changes  ← Extra scrutiny on security
   ↓
4. create test plan  ← Includes security test scenarios
   ↓
5. [Implement tests]  ← Including security-specific tests
   ↓
6. [Execute tests]  ← Including penetration testing
   ↓
7. [Create PR]  ← Mark as security-sensitive
```

**Additional Steps:**
- Security review by security team
- Penetration testing
- Security scan with tools
- Documentation of attack vectors and mitigations

### Variation 4: Multi-Component Change

**Scenario:** Change affects runtime, installer, tools, and tests

**Workflow:**
```
1. start work on {work item}
   ↓
2. [Implement changes in all components]
   ↓
3. review my changes  ← Searches all components for missed changes
   ↓
4. create test plan  ← Tests all components and integrations
   ↓
5. [Implement tests for all components]
   ↓
6. [Execute all tests]
   ↓
7. [Create PR]
```

**Critical:** Code review protocol searches entire codebase for similar patterns that might need the same fix.

---

## Phase Integration

### How Phases Connect

**Documentation Flow:**
```
Phase 1 (Start Work)
  ↓ Creates
docs/notes/{Component}-{Feature}.md
  ↓ Updated by
Phase 3 (Code Review)
  ↓ Adds "Change Review Summary"
docs/notes/{Component}-{Feature}.md
  ↓ Referenced by
Phase 4 (Create Test Plan)
  ↓ Updates with test plan link
docs/notes/{Component}-{Feature}.md
```

**Test Flow:**
```
Phase 4 (Create Test Plan)
  ↓ Creates
docs/testing/{Component}-{Feature}.md
  ↓ Implemented in
Phase 5 (Implement Tests)
  ↓ Executed in
Phase 6 (Execute Tests)
  ↓ Results referenced in
Phase 7 (Pull Request)
```

**Prerequisites Chain:**
```
Phase 1 (Start Work)
  → No prerequisites

Phase 3 (Code Review)
  → Requires: Feature documentation exists

Phase 4 (Create Test Plan)
  → Requires: Change review completed
  → Requires: Feature documentation exists

Phase 7 (Pull Request)
  → Requires: All tests passing
  → Requires: Documentation complete
  → Requires: Code review complete
```

---

## Quality Gates

### Gate 1: Documentation Created (After Phase 1)

**Check:**
- [ ] Feature documentation exists in `docs/notes/`
- [ ] Work item details included
- [ ] Problem statement documented (or TODO)
- [ ] Solution approach documented (or TODO)
- [ ] Implementation plan outlined

**If Failed:** Cannot proceed to implementation

---

### Gate 2: Code Review Complete (After Phase 3)

**Check:**
- [ ] Change Review Summary added to documentation
- [ ] All components reviewed
- [ ] Security vulnerabilities addressed
- [ ] Codebase searched for missed changes
- [ ] Test status checked
- [ ] Merge readiness assessed

**If Failed:** Fix issues before creating test plan

---

### Gate 3: Test Plan Created (After Phase 4)

**Check:**
- [ ] Test plan exists in `docs/testing/`
- [ ] Test scenarios comprehensive
- [ ] Test cases cover all changes
- [ ] Automated test specifications defined
- [ ] Manual test procedures defined
- [ ] Feature documentation updated with test plan link

**If Failed:** Cannot proceed to test implementation

---

### Gate 4: Tests Passing (After Phase 6)

**Check:**
- [ ] All automated tests passing
- [ ] All manual tests passed
- [ ] Test results documented in test plan
- [ ] All defects resolved
- [ ] Test summary complete

**If Failed:** Cannot create pull request

---

## Common Issues and Solutions

### Issue 1: Missing Documentation

**Symptom:** Code review protocol fails with "PREREQUISITE MISSING"

**Cause:** Feature documentation doesn't exist

**Solution:**
```
run: create documentation
then: review my changes
```

---

### Issue 2: Change Review Not Complete

**Symptom:** Test plan protocol fails with prerequisite error

**Cause:** Change Review Summary not in feature documentation

**Solution:**
```
run: review my changes
then: create test plan
```

---

### Issue 3: Tests Failing

**Symptom:** Cannot create pull request, tests failing

**Cause:** Implementation has bugs or test cases have issues

**Solution:**
1. Review test failure output
2. Fix implementation or test cases
3. Rerun tests
4. Update test plan with results

---

### Issue 4: Incomplete Test Coverage

**Symptom:** Code review finds untested scenarios

**Cause:** Test plan doesn't cover all changes

**Solution:**
1. Review Change Review Summary
2. Update test plan with missing scenarios
3. Implement missing tests
4. Execute tests
5. Update test results

---

## Best Practices

### 1. Create Documentation Early
- Run "start work on {number}" or "create documentation" before coding
- Don't wait until the end to document
- Documentation helps clarify requirements and approach

### 2. Update Documentation Continuously
- Fill in TODO sections as you implement
- Document decisions as you make them
- Track questions and get them answered
- Keep implementation plan current

### 3. Run Code Review When Implementation Complete
- Don't skip code review
- Address all issues found
- Let review find missed changes
- Use review to validate completeness

### 4. Create Comprehensive Test Plan
- Let protocol analyze end-to-end flows
- Don't just test changed code
- Test integration points
- Include security and performance tests

### 5. Execute All Tests
- Don't skip manual tests
- Document all test results
- Fix defects before pull request
- Keep test plan updated with results

### 6. Link Everything
- Link work items to branches
- Link branches to pull requests
- Link documentation to test plans
- Link test results to pull requests

### 7. Follow Prerequisites
- Each phase has prerequisites for a reason
- Don't skip phases
- Prerequisites ensure quality
- Skipping phases causes rework

---

## Protocol Reference

### Protocols Used in Lifecycle

1. **Start Work:** `.claude/protocols/start_work.md`
2. **Create Documentation:** `.claude/protocols/create_documentation.md`
3. **Code Review:** `.claude/protocols/change_review.md`
4. **Create Test Plan:** `.claude/protocols/create_test_plan.md`

### Trigger Phrases Quick Reference

| Phase | Trigger Phrase | Protocol |
|-------|---------------|----------|
| 1 | `start work on {number}` | start_work.md |
| 1 | `create documentation` | create_documentation.md |
| 3 | `review my changes` | change_review.md |
| 4 | `create test plan` | create_test_plan.md |

---

## Examples

### Example 1: New Feature - Batch Size Auto-Scaling

**Phase 1:**
```
User: start work on 120000
Claude: [Creates branch features/120000-batch-size-autoscaling]
        [Creates docs/notes/BatchProcessing-AutoScaling.md]
        [Suggests implementation based on similar code]
```

**Phase 2:**
```
User: [Implements auto-scaling logic]
      [Updates documentation with details]
      [Commits changes]
```

**Phase 3:**
```
User: review my changes
Claude: [Reviews implementation]
        [Searches for similar batch processing code]
        [Checks for performance implications]
        [Updates docs with Change Review Summary]
```

**Phase 4:**
```
User: create test plan
Claude: [Analyzes auto-scaling changes]
        [Creates performance test scenarios]
        [Defines load testing procedures]
        [Creates docs/testing/BatchProcessing-AutoScaling.md]
        [Updates feature docs with test plan link]
```

**Phase 5-7:**
```
User: [Implements tests]
      [Runs performance tests]
      [Documents results]
      [Creates pull request]
```

---

### Example 2: Security Fix - Password Exposure

**Phase 1:**
```
User: start work on 115163
Claude: [Creates branch features/115163-plaintext-password-fix]
        [Creates docs/notes/KeyDatabaseLegacy-PasswordSecurityFix.md]
        [Identifies security vulnerability]
```

**Phase 2:**
```
User: [Fixes password exposure in all components]
      [Updates documentation]
      [Commits security fix]
```

**Phase 3:**
```
User: review my changes
Claude: [Extra security scrutiny]
        [Searches ALL components for password exposure]
        [Finds 4 components affected]
        [Verifies fix in all locations]
        [Updates docs with comprehensive Change Review Summary]
```

**Phase 4:**
```
User: create test plan
Claude: [Creates security-focused test plan]
        [Includes penetration test scenarios]
        [Defines security verification procedures]
        [Creates docs/testing/KeyDatabaseLegacy-PasswordSecurityFix.md]
```

**Phase 5-7:**
```
User: [Implements security tests]
      [Runs penetration tests]
      [Verifies no password exposure]
      [Creates PR with security tag]
```

---

## Metrics and Tracking

### Lifecycle Metrics

Track these metrics for each feature/change:

**Documentation:**
- Time to create initial docs
- Number of TODO sections at start
- Number of TODO sections at review
- Documentation completeness score

**Code Review:**
- Number of components reviewed
- Number of security issues found
- Number of missed changes found
- Time to complete review

**Testing:**
- Number of test scenarios
- Number of test cases
- Test coverage percentage
- Time to implement tests
- Time to execute tests
- Number of defects found

**Overall:**
- Time from start to PR
- Number of rework cycles
- Pull request approval time
- Merge success rate

---

## Continuous Improvement

### Learning from Lifecycle

After each feature/change:

**What Went Well:**
- Which phases were smooth?
- What documentation was most helpful?
- Which tests caught issues?

**What Could Improve:**
- Which phases had issues?
- What documentation was missing?
- What tests were missing?

**Actions:**
- Update protocols based on learnings
- Add to Best Practices
- Update quality gates
- Improve documentation templates

---

**Last Updated:** 2026-01-02
**Maintainer:** Matthew Whited
