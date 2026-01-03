# Task Management Protocol

## Document Information

| Field | Value |
|-------|-------|
| Version | 1.1 |
| Last Updated | 2026-01-01 |
| Status | Active |

---

## Overview

This protocol defines the complete lifecycle for managing development tasks from initial request through implementation, documentation, and cleanup.

### Task Lifecycle

```
Request → Track → Review → Propose → Plan → Implement → Document → Complete → Cleanup
```

---

## Phase 1: Add to Tracking

### 1.1 Receive Request

When a new feature request or bug report is received:

1. **Clarify requirements** - Ask questions if unclear
2. **Assess scope** - Is this a quick fix or multi-phase feature?
3. **Identify dependencies** - What existing code/systems are affected?

### 1.2 Add to TODO.md

Add new item to `TODO.md` under "Active Work" section:

```markdown
### N. [Feature Name]

**Status:** `PROPOSED`
**Priority:** [High/Medium/Low]
**Document:** [docs/proposals/{feature}.md](./docs/proposals/{feature}.md)

[Brief 1-2 sentence summary]

**Key Deliverables:**
- [ ] Deliverable 1
- [ ] Deliverable 2
- [ ] Deliverable 3
```

### 1.3 Update Quick Reference Tables

Add to appropriate priority table:

```markdown
### [Priority] Priority
| Item | Status | Owner |
|------|--------|-------|
| [Feature Name] | Proposed | [Owner] |
```

Add to Pending Proposals table:

```markdown
| [Feature Name] | {feature}.md | PROPOSED | [Priority] |
```

---

## Phase 2: Review Feature Request

### 2.1 Understand the Problem

- What problem does this solve?
- Who is affected?
- What is the current workaround (if any)?
- What is the business impact?

### 2.2 Research Existing Code

```bash
# Search for related code
grep -rn "RelatedTerm" --include="*.cs" src/
grep -rn "RelatedTerm" --include="*.tsx" src/

# Find related files
find src -name "*Related*" -type f

# Check existing patterns
cat src/path/to/similar/implementation.cs
```

### 2.3 Identify Affected Areas

Document:
- Which layers are affected (Controller, Service, Repository, Model)?
- Which files need modification?
- Are there database changes needed?
- Are there frontend changes needed?

### 2.4 Assess Complexity

| Complexity | Criteria | Approach |
|------------|----------|----------|
| **Simple** | Single file, < 50 lines | Direct implementation |
| **Medium** | 2-5 files, single layer | Brief proposal |
| **Complex** | Multiple layers, new patterns | Full proposal document |
| **Major** | Architecture change, breaking changes | ADR + detailed proposal |

---

## Phase 3: Propose Changes

### 3.1 Create Proposal Document

For Medium+ complexity, create `docs/proposals/{feature}.md`:

```markdown
# Proposal: [Feature Name]

**Status:** Planning
**Priority:** [HIGH/MEDIUM/LOW]
**Created:** YYYY-MM-DD

---

## Problem Statement

[Describe the problem being solved]

---

## Proposed Solution

[High-level solution description]

### Option A: [Approach Name]
- Pros: ...
- Cons: ...

### Option B: [Alternative] (if applicable)
- Pros: ...
- Cons: ...

### Recommended: [Option X]

[Rationale for recommendation]

---

## Implementation Plan

### Phase 1: [Phase Name]
- [ ] Task 1
- [ ] Task 2

### Phase 2: [Phase Name]
- [ ] Task 3
- [ ] Task 4

---

## Files to Modify

| File | Change |
|------|--------|
| path/to/file.cs | Description of change |

---

## Testing Strategy

[How will this be tested?]

---

## Rollback Plan

[How to revert if issues arise]

---

*Created: YYYY-MM-DD*
```

### 3.2 Get Approval

- Present proposal to stakeholders
- Address questions and concerns
- Update proposal based on feedback
- Update status in TODO.md: `PROPOSED` → `APPROVED`

---

## Phase 4: Create Test Plan

### 4.1 Identify Test Scenarios

| Category | Test Cases |
|----------|------------|
| **Happy Path** | Normal successful operations |
| **Edge Cases** | Boundary conditions, empty inputs |
| **Error Cases** | Invalid inputs, system failures |
| **Security** | Authorization, data scoping |
| **Performance** | Load, concurrency (if applicable) |

### 4.2 Document Test Plan

Add to proposal or create separate test document:

```markdown
## Test Plan

### Unit Tests
| Test | Description | Expected Result |
|------|-------------|-----------------|
| Test_ValidInput_ReturnsSuccess | ... | ... |
| Test_InvalidInput_ReturnsError | ... | ... |

### Integration Tests
| Test | Description | Expected Result |
|------|-------------|-----------------|
| ... | ... | ... |

### Manual Testing
| Scenario | Steps | Expected Result |
|----------|-------|-----------------|
| ... | 1. ... 2. ... | ... |
```

### 4.3 Define Acceptance Criteria

- [ ] All unit tests pass
- [ ] All integration tests pass
- [ ] Manual testing scenarios verified
- [ ] No regression in existing functionality
- [ ] Performance within acceptable limits

---

## Phase 5: Implement Feature

### 5.1 Update Status

In TODO.md, change status:
```markdown
**Status:** `IN_PROGRESS`
```

### 5.2 Implementation Order

1. **Models/Entities** - Data structures first
2. **Repository** - Data access layer
3. **Service** - Business logic
4. **Controller** - API endpoints
5. **Attributes/Filters** - Metadata and cross-cutting concerns
6. **Frontend** - UI components (if applicable)

### 5.3 Follow Coding Standards

- Add XML documentation to public APIs
- Follow existing patterns in codebase
- Use expression-bodied members for simple methods
- Apply appropriate security attributes
- Add metadata attributes for OpenAPI extensions

### 5.4 Run Tests

```bash
# Build
dotnet build

# Run tests
dotnet test

# Manual verification
dotnet run --project src/GreenOnion.API
```

### 5.5 Self-Review Checklist

- [ ] Code compiles without warnings
- [ ] All tests pass
- [ ] Security attributes applied correctly
- [ ] XML documentation added
- [ ] No hardcoded values (use configuration)
- [ ] Error handling appropriate
- [ ] Logging added where needed

---

## Phase 6: Update Documents

### 6.1 Architecture Documentation

If feature introduces new patterns:
- Update relevant `docs/architecture/*.md`
- Add diagrams if helpful (PlantUML)

### 6.2 API Documentation

If new endpoints or models:
- Add XML documentation to code
- Update `docs/swagger-extensions.md` if new extensions
- Verify OpenAPI spec is generated correctly

### 6.3 CLAUDE.md Updates

If feature affects development workflow:
- Add to "Recent Implementations" section
- Update file counts/locations if changed
- Add any new patterns to remember

### 6.4 Frontend Guide

If frontend integration needed:
- Update or create guide in `docs/`
- Include code examples
- Document expected behavior

---

## Phase 7: Add Change History

### 7.1 Change History vs Documentation

**Important distinction:**

| File Type | Contains | Does NOT Contain |
|-----------|----------|------------------|
| `docs/changes/{feature}.md` | What changed, files modified, decisions made | How to use, examples, guides |
| `docs/architecture/*.md` | How it works, patterns, design | Change history |
| `docs/*.md` guides | How to use, examples, integration | Implementation details |

Change history is a **record of what was done**, not documentation of how to use the feature.

### 7.2 Create Change History File

Create `docs/changes/{feature}.md`:

```markdown
# Change: [Feature Name]

**Completed:** YYYY-MM-DD
**Category:** [Feature/Bug Fix/Enhancement/Documentation]

---

## Summary

[1-2 sentence description of what was implemented]

## What Changed

### Files Created
- `path/to/new/file.cs`
- `path/to/another/file.cs`

### Files Modified
- `path/to/existing/file.cs`

## Key Decisions

- [Decision 1 and rationale]
- [Decision 2 and rationale]

## Documentation

For usage and implementation details, see:
- [Architecture Doc](../architecture/relevant.md)
- [API Reference](../swagger-extensions.md)
- [Guide](../relevant-guide.md)

---

*Completed: YYYY-MM-DD*
```

### 7.3 Update Feature Documentation

The actual "how to use" documentation goes in the appropriate location:

| Content Type | Location |
|--------------|----------|
| Architecture/design | `docs/architecture/{feature}.md` |
| API extensions | `docs/swagger-extensions.md` |
| Frontend guides | `docs/{feature}-guide.md` |
| Data sync | `docs/data-sync/*.md` |
| Protocols | `.claude/protocols/*.md` |

---

## Phase 8: Cleanup Tracking

### 8.1 Update TODO.md

1. **Remove from Active Work** section entirely

2. **Remove from Quick Reference** tables (High/Medium/Low Priority)

3. **Remove from Pending Proposals** table (strike-through or delete row)

4. **DO NOT add to "Recently Completed"** - that section is only for quick reference during active development sessions. Once changes are logged in `docs/changes/`, the item is fully tracked there.

**Rationale:** TODO.md should only contain **active work**. Completed work is tracked in:
- `docs/changes/{descriptive-name}.md` - Change summary (what was done)
- `docs/features/{feature}.md` - Feature documentation (how it works)

### 8.2 Move Proposal to Features Folder

Once implemented and change history created, move the proposal to the features archive:

```bash
# Move completed proposal to features folder
mv docs/proposals/{feature}.md docs/features/{feature}.md
```

**Folder purposes:**
- `docs/proposals/` - Active proposals under consideration or in progress
- `docs/features/` - Completed/implemented features (archive of what was built)

Update the status in the moved file to reflect completion:
```markdown
**Status**: ✅ Implemented (YYYY-MM-DD)
```

### 8.3 Final Verification

- [ ] TODO.md updated
- [ ] Change history created
- [ ] CHANGELOG.md updated
- [ ] Proposal document removed (if applicable)
- [ ] CLAUDE.md updated (if needed)
- [ ] All tests passing
- [ ] Documentation complete

---

## Quick Reference: Task States

```
PROPOSED → APPROVED → IN_PROGRESS → COMPLETED
    ↓
  BLOCKED (if dependencies)
    ↓
  REJECTED (if not approved)
```

## Quick Reference: File Locations

| Purpose | Location |
|---------|----------|
| Active tracking | `TODO.md` |
| Active proposals | `docs/proposals/{feature}.md` |
| Completed features | `docs/features/{feature}.md` |
| Change summaries | `docs/changes/{descriptive-name}.md` |
| Architecture | `docs/architecture/*.md` |
| API docs | `docs/swagger-extensions.md` |
| Protocols | `.claude/protocols/*.md` |
| Memory | `CLAUDE.md` |

### Change Summary Naming Convention

Change summary filenames should be **descriptive of the change set**, not just the feature name:

| Good | Bad |
|------|-----|
| `2026-01-query-model-extensions.md` | `query-models.md` |
| `2025-12-31-bulk-actions-fixes.md` | `bulk-actions.md` |
| `entity-based-filter-linking.md` | `filters.md` |
| `external-sync-readonly-full-stack.md` | `sync.md` |

**Format:** `[optional-date-]{descriptive-change-name}.md`

## Quick Reference: Status Updates

| When | Update TODO.md Status To |
|------|--------------------------|
| Request received | `PROPOSED` |
| Approved for work | `APPROVED` |
| Implementation started | `IN_PROGRESS` |
| Waiting on dependency | `BLOCKED` |
| Fully complete | `COMPLETED` (then move to Recently Completed) |

---

## Related Protocols

| Protocol | Purpose |
|----------|---------|
| [security-audit.md](./security-audit.md) | Security verification |
| [documentation-standards.md](./documentation-standards.md) | Doc organization |
| [template-and-swagger-documentation.md](./template-and-swagger-documentation.md) | Template maintenance |

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.1 | 2026-01-01 | Added features folder for completed proposals; clarified change summary naming; removed "Recently Completed" requirement |
| 1.0 | 2025-12-31 | Initial protocol |
