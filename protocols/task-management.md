# Task Management Protocol

This protocol defines the standard workflow for managing feature requests, bug fixes, and enhancements in the Green Onion application.

---

## Overview

Every task follows this lifecycle:

```
Request → Tracking → Analysis → Proposal → Test Plan → Implementation → Documentation → Cleanup
```

**Key Files:**
- `TODO.md` - Central task tracking
- `docs/epics/{category}/{feature}.md` - Feature proposals and plans
- `docs/changes/{feature}.md` - Change history (one file per feature)

---

## Phase 1: Add to Tracking

**Purpose:** Capture the request in TODO.md for visibility and prioritization.

### Steps

1. **Identify the category:**
   - Security issue → CRITICAL priority
   - Backend required → Note dependency
   - Frontend only → Can proceed independently
   - Template change → Affects generated code

2. **Add entry to TODO.md:**

```markdown
### {Priority} - {Category}
{N}. **{Feature Name}** - {Brief description}
   - {Key requirement 1}
   - {Key requirement 2}
   - See: `docs/epics/{category}/{feature-slug}.md`
```

3. **Priority Levels:**
   - **CRITICAL** - Security, data loss, blocking issues
   - **HIGH** - Core functionality, user-facing bugs
   - **MEDIUM** - Enhancements, quality improvements
   - **LOW** - Nice-to-have, future considerations

### Example Entry

```markdown
### High Priority - Infrastructure
2. **Entity-based DefinedFilter linking** - Replace path/instance with entityType
   - Add `entityType` field to DefinedFilter model
   - Update SimpleGenericGrid to use entityType
   - Enable cross-page filter sharing
   - See: `docs/epics/datagrid/entity-based-filters-plan.md`
```

---

## Phase 2: Review Feature Request

**Purpose:** Understand requirements, identify dependencies, assess complexity.

### Steps

1. **Gather context:**
   - Read existing documentation
   - Search codebase for related code
   - Check swagger.json for relevant metadata
   - Review related TODO.md entries

2. **Identify dependencies:**
   - Backend changes required?
   - Template changes vs. one-off code changes?
   - Other features that must complete first?
   - Breaking changes to existing functionality?

3. **Assess complexity:**
   - Files to modify (count)
   - Testing requirements
   - Documentation updates needed
   - Migration/rollback considerations

4. **Document findings in epic file:**

```markdown
# {Feature Name}

**Status**: Under Review
**Priority**: {HIGH/MEDIUM/LOW}
**Created**: {YYYY-MM-DD}

## Overview
{What this feature does and why it's needed}

## Dependencies
- Backend: {Yes/No} - {Details}
- Other features: {List}

## Complexity Assessment
- Files affected: {N}
- Breaking changes: {Yes/No}
- Migration required: {Yes/No}
```

---

## Phase 3: Propose Changes

**Purpose:** Design the solution with clear implementation steps.

### Steps

1. **Create/update epic document:**

```
docs/epics/{category}/{feature-slug}.md
```

2. **Document the proposal:**

```markdown
# {Feature Name} Implementation Plan

**Status**: Proposed
**Priority**: {Priority}
**Created**: {YYYY-MM-DD}

---

## Overview
{Problem statement and solution summary}

## Current State
{How things work now}

## Proposed Changes

### {Component/File 1}
{What changes and why}

### {Component/File 2}
{What changes and why}

## Implementation Steps

1. {Step 1 - specific action}
2. {Step 2 - specific action}
3. {Step 3 - specific action}

## API/Schema Changes
{If applicable - new fields, endpoints, metadata}

## Migration Strategy
{How to handle existing data/code}

## Backward Compatibility
{What breaks, what doesn't}
```

3. **Get user approval before proceeding**

---

## Phase 4: Create Test Plan

**Purpose:** Define how to verify the implementation works correctly.

### Steps

1. **Add test plan section to epic document:**

```markdown
## Test Plan

### Prerequisites
- {Required data/setup}

### Test Cases

| # | Scenario | Steps | Expected Result |
|---|----------|-------|-----------------|
| 1 | {Scenario name} | {Steps to execute} | {What should happen} |
| 2 | {Edge case} | {Steps} | {Expected} |
| 3 | {Error case} | {Steps} | {Expected error handling} |

### Verification Commands
```bash
npm run build          # Must pass
npm test               # If unit tests exist
```

### Manual Testing
1. {Navigation to test page}
2. {Actions to perform}
3. {Expected UI state}
```

2. **Consider edge cases:**
   - Empty states
   - Error conditions
   - Permission boundaries
   - Large data sets
   - Concurrent operations

---

## Phase 5: Implement the Feature

**Purpose:** Write the code following established patterns.

### Steps

1. **Update status in epic document:**
   ```markdown
   **Status**: In Progress
   ```

2. **Follow implementation order:**
   - Schema/API changes first (if backend)
   - Template changes (if generated code)
   - Component changes
   - Integration points
   - UI polish

3. **Coding standards:**
   - Follow patterns in ARCHITECTURE.md
   - Use existing utilities (don't reinvent)
   - Keep changes focused (no scope creep)
   - Add comments only where logic isn't self-evident

4. **Verify as you go:**
   ```bash
   npm run build    # After each significant change
   ```

5. **Track progress with TodoWrite tool:**
   - Mark tasks in_progress when starting
   - Mark completed immediately when done
   - Add discovered sub-tasks as needed

### Template Changes Checklist

If modifying Handlebars templates:

- [ ] Test with multiple entity types mentally
- [ ] Handle missing metadata gracefully
- [ ] Use correct helper syntax (see `docs/features/TEMPLATE_CONTEXT_REFERENCE.md`)
- [ ] Verify generated TypeScript compiles
- [ ] Update SCHEMA_METADATA_GUIDE.md if adding new x-* extensions

---

## Phase 6: Update Documentation

**Purpose:** Keep all documentation accurate and current.

### Required Updates

1. **Epic document** → Mark as complete:
   ```markdown
   **Status**: ✅ COMPLETE
   **Completed**: {YYYY-MM-DD}
   ```

2. **TODO.md** → Move to completed section:
   ```markdown
   ## ✅ HISTORICAL COMPLETIONS

   **{YYYY-MM-DD}**: {Feature Name} - {Brief description of what was done}
   ```

3. **SCHEMA_METADATA_GUIDE.md** → If new x-* extensions added

4. **ARCHITECTURE.md** → If new patterns established

5. **CLAUDE.md** → If workflow/commands changed

### Optional Updates

- `APPLICATION_SUMMARY.md` - If routes/components changed
- `API_CALLS.md` - If API patterns changed
- `STYLE_GUIDE.md` - If DataGrid patterns changed

---

## Phase 7: Add Change History

**Purpose:** Create permanent record of significant changes.

### Create Change Document

Create a dedicated change file for each feature:

```
docs/changes/{feature-slug}.md
```

**Template:**

```markdown
# {Feature Name}

**Date**: {YYYY-MM-DD}
**Type**: {Feature/Enhancement/Bug Fix/Refactor}
**Status**: Complete

---

## Summary
{One paragraph describing what was done and why}

## Files Modified

| File | Change |
|------|--------|
| `{path/to/file}` | {What changed} |

## Implementation Details

### {Section 1}
{Technical details}

### {Section 2}
{Technical details}

## Testing Performed
- {Test 1 and result}
- {Test 2 and result}

## Verification
- Build: ✅ Success ({N} modules, {N}s)
- Tests: ✅ Passing

## Impact
- **Breaking Changes**: {None/List}
- **Migration Required**: {No/Yes - details}
- **Rollback Plan**: {If applicable}

---

*Implemented by Claude Code on {YYYY-MM-DD}*
```

### For Minor Changes

For very minor changes (typo fixes, small config changes), a change document may not be necessary. Use your judgment - if it's worth documenting, create a file.

---

## Phase 8: Cleanup Tracking

**Purpose:** Keep TODO.md clean and accurate.

### Steps

1. **Remove completed item from pending sections**

2. **Add to Historical Completions:**
   ```markdown
   ## ✅ HISTORICAL COMPLETIONS

   **{YYYY-MM-DD}**: {Feature} - {Brief description}
   ```

3. **Renumber remaining items** (if using numbered lists)

4. **Update status sections:**
   - Update module counts if changed
   - Update build time if significantly different
   - Update quality status if applicable

5. **Archive epic document** (move to completed status, don't delete)

6. **Verify no orphaned references:**
   - Check that "See: docs/epics/..." links are valid
   - Remove references to completed work from pending sections

---

## Quick Reference Checklist

Use this checklist for each feature:

```markdown
### Feature: {Name}

#### Phase 1: Tracking
- [ ] Added to TODO.md with priority
- [ ] Created epic document path

#### Phase 2: Review
- [ ] Gathered context
- [ ] Identified dependencies
- [ ] Assessed complexity

#### Phase 3: Proposal
- [ ] Documented current state
- [ ] Proposed changes
- [ ] Listed implementation steps
- [ ] User approved

#### Phase 4: Test Plan
- [ ] Defined test cases
- [ ] Identified edge cases
- [ ] Listed verification commands

#### Phase 5: Implementation
- [ ] Code changes complete
- [ ] Build passes
- [ ] Manual testing done

#### Phase 6: Documentation
- [ ] Epic marked complete
- [ ] TODO.md updated
- [ ] Related docs updated

#### Phase 7: Change History
- [ ] Change file created: `docs/changes/{feature}.md`

#### Phase 8: Cleanup
- [ ] Removed from pending
- [ ] Added to completions
- [ ] Links verified
```

---

## Epic Categories

Organize epics by category:

| Category | Path | Examples |
|----------|------|----------|
| Security | `docs/epics/security/` | Permissions, auth, RBAC |
| DataGrid | `docs/epics/datagrid/` | Filters, columns, actions |
| Bulk Actions | `docs/epics/bulk-actions/` | Batch operations |
| External Sync | `docs/epics/external-sync/` | OneWorldSync integration |
| Templates | `docs/epics/templates/` | Code generation |
| Migration | `docs/epics/migration/` | V2 transition |
| API | `docs/epics/api/` | Endpoints, clients |

---

## Status Values

Use consistent status values across documents:

| Status | Meaning |
|--------|---------|
| `Proposed` | Idea documented, not approved |
| `Approved` | User approved, ready to start |
| `In Progress` | Currently being implemented |
| `Blocked` | Waiting on dependency |
| `Testing` | Implementation done, testing |
| `✅ COMPLETE` | Done and documented |
| `❌ CANCELLED` | Decided not to proceed |

---

*Protocol Version: 1.0*
*Created: 2025-12-31*
