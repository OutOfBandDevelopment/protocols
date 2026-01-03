# Protocol: Compare Documentation to Project Implementation

Compare existing rewrite review documentation against a target project to identify gaps, missing features, and create an implementation plan.

## Trigger
- User wants to compare docs to a new/rewritten project
- User asks "what's missing from the rewrite?"
- User wants to validate rewrite coverage

## Inputs Required
1. **Source Documentation**: Path to rewrite-review docs (e.g., `docs/rewrite-review/`)
2. **Target Project**: Path to project being compared (could be same repo or different)

## Phase 1: Load Documentation Baseline

### 1.1 Parse action-index.md
Extract all documented:
- Roles
- Screens per role
- Actions per screen
- Linked endpoint docs

### 1.2 Parse screen-index.md
Extract all documented:
- Screen routes
- Endpoints per screen
- Expected functionality

### 1.3 Parse roles.md
Extract:
- Role IDs and names
- Permissions matrix
- Authorization patterns

### 1.4 Parse Feature Docs
For each feature documentation file:
- Extract endpoints
- Extract request/response models
- Extract business rules

## Phase 2: Analyze Target Project

### 2.1 Backend Analysis
```
Task: Explore target project backend:
- List all controllers and endpoints
- Identify request/response models
- Find authorization attributes
- Map to feature areas
```

### 2.2 Frontend Analysis
```
Task: Explore target project frontend:
- List all routes
- Identify page components
- Find API calls
- Map state management
```

### 2.3 Create Target Inventory
Output: Complete list of implemented features in target

## Phase 3: Gap Analysis

### 3.1 Endpoint Comparison
```
For each endpoint in documentation:
  Search target for matching:
    - Route pattern
    - HTTP method
    - Similar naming

  Categorize as:
    - FOUND: Exact or close match
    - PARTIAL: Similar but different
    - MISSING: Not found
```

### 3.2 Screen Comparison
```
For each screen in documentation:
  Search target for:
    - Route definition
    - Page component
    - Required functionality

  Categorize as:
    - COMPLETE: All features present
    - PARTIAL: Some features missing
    - MISSING: Screen not implemented
```

### 3.3 Role Comparison
```
For each role in documentation:
  Verify in target:
    - Role exists
    - Permissions match
    - Authorization enforced
```

### 3.4 Feature Comparison
```
For each documented business rule:
  Search target for:
    - Implementation
    - Tests covering rule

  Categorize as:
    - IMPLEMENTED
    - PARTIALLY_IMPLEMENTED
    - NOT_IMPLEMENTED
```

## Phase 4: Generate Gap Report

### 4.1 Create gap-analysis.md
```markdown
# Gap Analysis Report

Generated: {date}
Source Docs: {path}
Target Project: {path}

## Summary

| Category | Documented | Found | Partial | Missing |
|----------|------------|-------|---------|---------|
| Endpoints | X | X | X | X |
| Screens | X | X | X | X |
| Features | X | X | X | X |

## Coverage: X%

---

## Missing Endpoints

| Endpoint | Feature | Priority | Notes |
|----------|---------|----------|-------|
| POST /api/product/CreateProduct | Product Mgmt | High | Core CRUD |
| ... | ... | ... | ... |

## Partial Implementations

| Feature | Missing Aspects | Priority |
|---------|-----------------|----------|
| Product Upload | OCR extraction not found | Medium |
| ... | ... | ... |

## Missing Screens

| Screen | Route | Priority | Dependencies |
|--------|-------|----------|--------------|
| Product Approval | /product-approval | High | None |
| ... | ... | ... | ... |

## Missing Business Rules

| Rule | Feature | Impact |
|------|---------|--------|
| GTIN must be unique | Products | Data integrity |
| ... | ... | ... |
```

## Phase 5: Generate Implementation Plan

### 5.1 Prioritize Gaps
```
Priority criteria:
- HIGH: Core functionality, blocks other features
- MEDIUM: Important but not blocking
- LOW: Nice to have, edge cases
```

### 5.2 Create implementation-plan.md
```markdown
# Implementation Plan

## Phase 1: Critical Missing Features
Estimated effort: X

### 1.1 {Feature Name}
**Gap**: {description}
**Endpoints needed**:
- POST /api/...
- GET /api/...

**Implementation steps**:
1. Create controller
2. Create service
3. Create repository method
4. Create frontend page
5. Add to navigation
6. Test

**Acceptance criteria**:
- [ ] Endpoint responds correctly
- [ ] UI matches wireframe
- [ ] Role permissions enforced
- [ ] Matches documented behavior

---

## Phase 2: Secondary Features
...

## Phase 3: Polish & Edge Cases
...

## Total Gaps Summary
- Critical: X items
- Medium: X items
- Low: X items
```

### 5.3 Create Tracking Checklist
```markdown
# Implementation Tracking

## Endpoints
- [ ] POST /api/product/CreateProduct
- [ ] GET /api/product/GetProductDataByID
- ...

## Screens
- [ ] /product-management/create
- [ ] /product-management/approval
- ...

## Business Rules
- [ ] GTIN uniqueness validation
- [ ] Role-based filtering
- ...
```

## Phase 6: Output Files

Create in target project or docs location:

```
docs/rewrite-review/
├── gap-analysis.md           # What's missing
├── implementation-plan.md    # How to close gaps
└── tracking/
    ├── endpoints.md          # Endpoint checklist
    ├── screens.md            # Screen checklist
    └── business-rules.md     # Rule checklist
```

## Usage Examples

### Example 1: Same Repo Comparison
```
User: Compare docs/rewrite-review to the new-api branch

Claude:
1. Load docs from docs/rewrite-review/
2. Checkout new-api branch (or analyze in parallel)
3. Compare endpoints, screens, features
4. Generate gap report
```

### Example 2: Cross-Repo Comparison
```
User: Compare this documentation to /path/to/new-project

Claude:
1. Load docs from current location
2. Explore /path/to/new-project
3. Compare and generate report
```

### Example 3: Partial Comparison
```
User: Just compare the Product Management features

Claude:
1. Load only product-management/ docs
2. Analyze only product-related code in target
3. Generate focused gap report
```

## Output Checklist

- [ ] Gap analysis report generated
- [ ] Implementation plan created
- [ ] Tracking checklists ready
- [ ] Priority assigned to all gaps
- [ ] Dependencies identified

## Automation Opportunities

For repeated comparisons, consider:
1. Script to extract endpoint list from code
2. Script to parse documentation structure
3. Diff tool for model comparisons
4. CI integration for coverage tracking
