# Create Documentation Protocol

## Trigger Phrases
- "create documentation"
- "create docs"
- "document this feature"
- "document this change"
- "create feature documentation"

## Overview

This protocol creates comprehensive feature/change documentation in `docs/notes/`. This should be run early in the development process, ideally right after starting work on a feature or after making changes that need documentation.

**When to Use:**
- After starting work on a new feature/bug
- After making changes that need documentation
- Before running change review (documentation is required for review)

**Note:** This protocol creates the initial documentation structure. The change review and test plan protocols will add their sections to this documentation.

## Protocol Steps

### 1. Determine Documentation Context

**Actions:**
- [ ] Check current git branch
- [ ] Extract work item number from branch name (if present)
- [ ] Check git status and log for changes
- [ ] Identify if this is new documentation or update

**Commands:**
```bash
git branch --show-current
git status --short
git log --oneline -10
```

**Extract Information:**
- Branch name and type (features/, bugs/, tasks/)
- Work item number (if in branch name)
- Changed files (if any)

**Output:**
```
📋 DOCUMENTATION CONTEXT

Branch: {branch-name}
Work Item: {#number} (if found)
Type: {Feature|Bug|Task|Change}
Changed Files: {count}
```

---

### 2. Fetch Work Item Details (if available)

**Actions:**
- [ ] If work item number found, fetch from Azure DevOps
- [ ] Extract title, description, acceptance criteria
- [ ] Get related work items and comments
- [ ] If no work item, ask user for basic information

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

**If No Work Item Found:**
```
ℹ️ No work item number found in branch name.

Please provide:
1. Work item number (if exists): {number}
2. Brief description of feature/change: {description}
3. Type: Feature | Bug | Security Fix | Refactoring | Other

Or type "skip" to create documentation without work item details.
```

**Output:**
```
📋 WORK ITEM DETAILS

#{number}: {title}
Type: {type}
Priority: {priority}
Description: {description}
Acceptance Criteria: {criteria}
```

---

### 3. Analyze Changed Files (if any)

**Actions:**
- [ ] Check git diff for changed files
- [ ] Identify components affected
- [ ] Search for similar patterns
- [ ] Note configuration or database changes

**Commands:**
```bash
# Get changed files
git diff --name-only HEAD

# Get diff stats
git diff --stat

# Search for similar code
grep -r "{pattern}" --include="*.cs"
```

**Component Identification:**
```
Source/CadLink.Server/         → Server Runtime
Source/CadLink.Client/         → Client Runtime
Source/CadLink.Common/         → Shared Runtime
Source/Tools/Installers/       → Installers
Source/Tools/DbKeyCustomAction/→ Installer Custom Action
InternalTools/                 → Internal Tools
Tests/                         → Tests
```

**Output:**
```
🔍 CODE ANALYSIS

Components Affected:
- {Component 1}
- {Component 2}

Files Changed:
- {file path} - {component}
- {file path} - {component}

Similar Features:
- {Feature name} in {files}
```

---

### 4. Determine Documentation File Name

**Actions:**
- [ ] Determine appropriate file name
- [ ] Check for existing documentation
- [ ] Confirm with user if multiple options

**Naming Conventions:**
```
{Component}-{Feature}.md
Examples:
- KeyDatabaseLegacy-PasswordSecurityFix.md
- DatabaseEncryption-SEEMigration.md
- BatchProcessing-AutoScaling.md

OR

{WorkItem}-{ShortDescription}.md
Examples:
- 115163-PlaintextPasswordFix.md
- 120000-BatchSizeAutoscaling.md
```

**File Name Generation:**
1. If work item exists: Use both formats, prefer component-feature
2. Extract component from changed files
3. Extract feature from work item title or user description
4. Convert to PascalCase with hyphens

**Check for Existing Documentation:**
```bash
# Check if file exists
ls docs/notes/*.md | grep -i "{keywords}"
```

**If Existing Documentation Found:**
```
ℹ️ Found existing documentation:
- docs/notes/{existing-file}.md

Options:
1. Update existing documentation
2. Create new documentation file
3. Cancel

What would you like to do?
```

**Output:**
```
📝 Documentation File: docs/notes/{filename}.md
Status: {New|Update Existing}
```

---

### 5. Create Documentation Structure

**Actions:**
- [ ] Generate comprehensive documentation file
- [ ] Include all required sections
- [ ] Mark sections as TODO where information isn't available yet
- [ ] Leave placeholders for change review and test plan sections

**Documentation Template:**

```markdown
# {Feature/Change Title} (Work Item #{number})

## Overview

{Brief description of the feature/change and why it was needed}

**Status:** 🚧 IN PROGRESS

## Branch Information

**Branch Name:** `{branch-name}`
**Work Item:** #{work-item-id} - {title}
**Base Branch:** `{base-branch}`
**Started:** {date}
**Last Updated:** {date}

## Work Item Details

**Type:** {Feature|Bug|Task|Security Fix}
**Priority:** {priority}
**State:** {state}
**Assigned To:** {name}

**Description:**
{full description from work item}

**Acceptance Criteria:**
{list acceptance criteria}
1. {criterion 1}
2. {criterion 2}

**Related Work Items:**
- Parent: #{number} - {title}
- Related: #{number} - {title}

## Change Review Summary

**Note:** This section will be populated when "review my changes" protocol is run.

**Status:** ⏳ Pending Review

---

## Problem Statement

{What problem does this solve? What was broken/missing?}

**TODO:** Refine problem statement as implementation progresses

## Solution

{How does the change address the problem?}

**TODO:** Update with detailed solution as design is finalized

## Changes Made

**TODO:** Document actual changes here as implementation progresses

### Files Modified

List all changed files with:
- File path
- Purpose of changes
- Before/after code examples (if significant)

### Components Affected

**Runtime Components:**
- {Component} - {changes}

**Installer Components:**
- {Component} - {changes}

**Internal Tools:**
- {Component} - {changes}

**Tests:**
- {Component} - {changes}

## Potentially Impacted Code

**Note:** Initial analysis. Will be refined during implementation and code review.

### Components
{List components from initial analysis}

### Files
{List files from search}

### Similar Features
{List similar features found}

## Implementation Plan

**TODO:** Break down implementation into steps

**Steps:**
1. [ ] {Step 1}
2. [ ] {Step 2}
3. [ ] {Step 3}

**Implementation Notes:**
{Notes about approach, patterns to use, etc.}

## Impact Analysis

**TODO:** Document impact as changes are made

### Functionality Affected
{What existing functionality is impacted?}

### Breaking Changes
{Are there any breaking changes? Yes/No and details}

### Performance Implications
{Any performance impacts?}

### Security Implications
{Any security impacts?}

## Testing

**TODO:** Document testing approach

**See:** `docs/testing/{matching-filename}.md` for detailed test plan

**Test Plan Status:** ⏳ Not Created

### Unit Tests
{List unit tests to create/update}

### Integration Tests
{List integration tests to create/update}

### Manual Testing
{List manual test scenarios}

## Deployment

**TODO:** Document deployment requirements

### Build Requirements
{What needs to be built/rebuilt?}

### Deployment Steps
{How to deploy this change?}

### Rollback Procedure
{How to rollback if needed?}

## Use Cases / Examples

**TODO:** Add real-world usage examples

### Use Case 1: {Scenario Name}
{Describe scenario and how feature addresses it}

### Use Case 2: {Scenario Name}
{Describe scenario and how feature addresses it}

## Security Considerations

**TODO:** Document security implications if applicable

{Security analysis, risks, mitigations}

## Performance Considerations

**TODO:** Document performance implications if applicable

{Performance analysis, benchmarks, optimizations}

## Configuration Changes

**TODO:** Document any configuration changes

{New settings, modified settings, migration notes}

## Database Changes

**TODO:** Document any database changes

{Schema changes, migration scripts, data impacts}

## Questions and Decisions

### Open Questions
{Track questions that need answers}

1. {Question 1}
2. {Question 2}

### Decisions Made

**{Date}:** {Decision}
- **Context:** {Why this decision was needed}
- **Decision:** {What was decided}
- **Rationale:** {Why this was chosen}
- **Alternatives Considered:** {What else was considered}

## Related Documentation

**Work Item:** https://dev.azure.com/cadwell/CadLink/_workitems/edit/{number}

**Related Features:**
{Links to related feature documentation}

**Code References:**
{Links to similar implementations or relevant code}

**External Resources:**
{Links to external documentation, RFCs, specifications}

## Troubleshooting

**TODO:** Document common issues and solutions

### Issue 1: {Issue Description}
**Symptom:** {What user sees}
**Cause:** {Why it happens}
**Solution:** {How to fix}

## Appendix

### Additional Information
{Any additional information that doesn't fit elsewhere}

---

**Document Version History**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | {date} | {name} | Initial documentation created |

---

**Next Steps:**
1. Implement feature/changes
2. Update this documentation with actual changes
3. Run "review my changes" to add Change Review Summary
4. Run "create test plan" to generate test documentation
5. Update status sections as work progresses
```

**Output:**
```
✅ DOCUMENTATION CREATED

File: docs/notes/{filename}.md
Status: New
Sections: {count}

Documentation includes:
✓ Overview and work item details
✓ Problem statement (TODO)
✓ Solution approach (TODO)
✓ Placeholder for Change Review Summary
✓ Placeholder for test plan reference
✓ Implementation plan (TODO)
✓ Questions and decisions tracking
✓ All standard sections

The documentation is ready for you to fill in details as you implement.
```

---

### 6. Create docs/testing Directory (if needed)

**Actions:**
- [ ] Check if docs/testing directory exists
- [ ] Create directory if needed
- [ ] Add .gitkeep if directory is empty

**Commands:**
```bash
# Check if directory exists
test -d docs/testing && echo "exists" || mkdir -p docs/testing

# Add .gitkeep if empty
test -z "$(ls -A docs/testing)" && touch docs/testing/.gitkeep
```

---

### 7. Provide Next Steps and Guidance

**Actions:**
- [ ] Summarize created documentation
- [ ] Provide clear next steps
- [ ] Offer assistance with implementation

**Summary Template:**

```markdown
✅ DOCUMENTATION CREATED SUCCESSFULLY

## Summary

**Documentation:** `docs/notes/{filename}.md`
**Work Item:** #{number} - {title}
**Type:** {Feature|Bug|Security Fix|etc.}
**Status:** 🚧 In Progress

## What Was Created

📄 **Feature Documentation Structure**
- Overview and work item details
- Problem statement (TODO)
- Solution approach (TODO)
- Implementation plan (TODO)
- Placeholder for Change Review Summary
- Placeholder for Test Plan reference
- All standard documentation sections

## What You Should Do Now

### 1. Fill in Implementation Details

As you implement the feature:
- Update "Problem Statement" with specific details
- Update "Solution" with your approach
- Update "Changes Made" with actual file changes
- Update "Implementation Plan" with completed steps
- Add "Questions and Decisions" as they arise

### 2. Update Documentation Continuously

Keep the documentation current:
- Mark TODOs as complete when you fill them in
- Add code examples showing before/after
- Document any issues or challenges you encounter
- Update the version history table

### 3. When Changes Are Complete

After implementation:
1. **Run:** "review my changes"
   - This will add comprehensive Change Review Summary
   - Validates completeness across components
   - Checks for missed changes

2. **Run:** "create test plan"
   - Generates detailed test plan in docs/testing/
   - Updates this documentation with test plan reference
   - Creates test scenarios based on actual changes

### 4. Before Creating Pull Request

Ensure:
- [ ] All TODO sections filled in
- [ ] Change Review Summary completed (by protocol)
- [ ] Test plan created (by protocol)
- [ ] All questions resolved or documented
- [ ] Documentation reflects actual implementation

## Quick Reference

**Edit Documentation:**
```bash
# Open in your editor
{editor} docs/notes/{filename}.md
```

**View Documentation:**
```bash
# View in terminal
cat docs/notes/{filename}.md
```

## How Can I Help?

I can help you with:
- "Show me {file path}" - Display relevant code
- "Search for {pattern}" - Find similar implementations
- "Help me implement {feature}" - Provide implementation guidance
- "Update documentation" - Update specific sections
- "review my changes" - Run comprehensive review (when ready)
- "create test plan" - Generate test plan (after review)

What would you like to work on next?
```

---

## Error Handling

### Documentation Already Exists

```
ℹ️ Documentation already exists: docs/notes/{filename}.md

Options:
1. Update existing documentation (recommended)
2. Create new documentation with different name
3. View existing documentation first
4. Cancel

What would you like to do?
```

### No Work Item and No Description Provided

```
⚠️ Insufficient Information

I need more information to create documentation:

Option 1: Provide work item number
"Work item #{number}"

Option 2: Provide manual details
Please provide:
- Brief description: {what is this feature/change}
- Type: Feature | Bug | Security Fix | Refactoring
- Why it's needed: {problem statement}

Option 3: Skip for now
Type "skip" and I'll create minimal documentation structure.
```

### Unable to Determine File Name

```
⚠️ Cannot determine documentation file name.

Please specify a file name:
"Create documentation as {ComponentName-FeatureName}"

Examples:
- DatabaseEncryption-PasswordSecurity
- BatchProcessing-AutoScaling
- 115163-PlaintextPasswordFix
```

---

## Integration with Other Protocols

### Workflow Integration

```
create documentation  ← Create docs first
    ↓
[Implement feature/changes]
    ↓
review my changes  ← Adds Change Review Summary to docs
    ↓
create test plan  ← Creates test plan and links from docs
    ↓
[Implement & run tests]
    ↓
[Create pull request]
```

### Called By Other Protocols

**start_work.md:**
- Calls this protocol to create initial documentation
- Passes work item details and branch information

**Change Review Prerequisites:**
- Requires documentation exists (created by this protocol)
- Updates documentation with Change Review Summary section

**Test Plan Prerequisites:**
- Requires documentation with Change Review Summary
- Updates documentation with test plan reference

---

## Documentation Updates by Other Protocols

### Change Review Protocol Updates

When "review my changes" runs, it will:
1. Find the feature documentation
2. Replace "Change Review Summary" section with actual review results
3. Update "Last Updated" date
4. Add version history entry

### Test Plan Protocol Updates

When "create test plan" runs, it will:
1. Find the feature documentation
2. Update "Testing" section with test plan reference
3. Update "Test Plan Status" to "Created"
4. Add link to `docs/testing/{matching-filename}.md`

---

## Example Usage

### Example 1: New Feature with Work Item

**User:** "create documentation"

**Protocol:**
1. ✓ Check branch: `features/115163-plaintext-password-fix`
2. ✓ Extract work item: 115163
3. ✓ Fetch work item from Azure DevOps
4. ✓ Analyze changed files (if any)
5. ✓ Propose filename: `KeyDatabaseLegacy-PasswordSecurityFix.md`
6. ✓ Create comprehensive documentation with all sections
7. ✓ Include work item details
8. ✓ Mark appropriate sections as TODO
9. ✓ Provide next steps

### Example 2: Bug Fix Without Work Item

**User:** "document this change"

**Protocol:**
1. ✓ Check branch: `bugfix/connection-timeout`
2. ✗ No work item found
3. ℹ️ Ask user for details
4. **User provides:** "Fix connection timeout in database pooling"
5. ✓ Analyze changed files
6. ✓ Propose filename: `DatabaseConnection-TimeoutFix.md`
7. ✓ Create documentation
8. ✓ Provide next steps

### Example 3: Update Existing Documentation

**User:** "create documentation"

**Protocol:**
1. ✓ Check branch
2. ✓ Search for existing docs
3. ℹ️ Found: `docs/notes/BatchProcessing-AutoScaling.md`
4. ℹ️ Ask user: Update existing or create new?
5. **User chooses:** Update existing
6. ✓ Read existing documentation
7. ✓ Offer to update specific sections
8. ✓ Update documentation

---

## Best Practices

1. **Create Documentation Early:** Right after starting work on a feature
2. **Keep It Current:** Update as you implement, don't wait until the end
3. **Be Specific:** Include file names, line numbers, exact changes
4. **Document Decisions:** Track why you made specific choices
5. **Track Questions:** Document unknowns and get them answered
6. **Use TODOs:** Mark sections to fill in later, but remove them before review
7. **Link Everything:** Link to work items, related docs, code references
8. **Think of Your Audience:** Write for future maintainers
9. **Update Version History:** Track significant documentation changes
10. **Run Protocols in Order:** Documentation → Review → Test Plan

---

## Customization

### Project-Specific Sections

Add sections specific to CadLink projects:

```markdown
## Installer Impact

**Server Installer:**
{Changes needed}

**Client Installer:**
{Changes needed}

**Custom Actions:**
{Changes to DbKeyCustomAction or other custom actions}

## Database Encryption

**Legacy Encryption (pre-5.3):**
{Impact on legacy encryption}

**SEE Encryption (5.3+):**
{Impact on SEE encryption}

## Configuration Template Updates

**CadLinkServerSettings.xml:**
{Changes to server config template}

**CadLinkClientSettings.xml:**
{Changes to client config template}
```

---

## Notes

- This protocol creates the foundation documentation
- Change review and test plan protocols enhance this documentation
- Keep documentation updated throughout implementation
- Documentation is a living document, not a one-time task
- Good documentation saves time during review and maintenance
