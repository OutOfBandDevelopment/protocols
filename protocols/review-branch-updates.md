# Protocol: Review Branch Updates

Review changes after pulling latest from API/UI branches, verify implementations, and update documentation.

> **IMPORTANT**: Always check planned features against PR changes and clean up completed/moved files. This step is critical and must not be skipped (see Phase 3.4).

## Trigger
- User says "pulled in latest" or "pulled latest changes"
- User says "review the changes" after a pull
- User mentions PR numbers that were merged

## Inputs Required
1. **Branch/PR Info**: Which branches were updated (API, UI, or both)
2. **PR Numbers** (optional): Specific PRs to review

## Phase 1: Identify Changes

### 1.1 Get Recent Commits
```bash
# For API submodule
cd GreenOnion.API && git log --oneline -10

# For UI submodule
cd GreenOnion.UI && git log --oneline -10
```

### 1.2 Review Commit Details
For each relevant commit:
```bash
git show <commit-hash> --stat
git show <commit-hash> -- "*.cs" "*.tsx" "*.ts"
```

### 1.3 Categorize Changes
Group changes by type:
- **New Controllers**: New API endpoints
- **New Models**: Query/Save models, DTOs
- **Model Extensions**: New fields on existing models
- **New Repositories**: Data access additions
- **UI Pages**: New generated or custom pages
- **UI Components**: Shared component changes

## Phase 2: Verify Frontend/Backend Alignment

### 2.1 For Each New Controller
Verify frontend has matching:
- API client in `GreenOnion.UI/src/api/GreenOnion/Clients/`
- Zod schema in `GreenOnion.UI/src/api/GreenOnion/Schema/`
- TypeScript model in `GreenOnion.UI/src/api/GreenOnion/Models/`

### 2.2 For Each Model Extension
Verify:
- C# property exists in `GreenOnion.Common/Models/`
- Zod schema has matching field with correct type
- TypeScript interface includes the field

### 2.3 Verify Data Mappings
For new Query models, compare:
```
C# Model Field          →  Zod Schema Field      →  TypeScript Type
string? ProductName     →  z.string().optional() →  string | undefined
DateTime? DateCreated   →  z.string().optional() →  string | undefined (ISO format)
int SchoolDistrictId    →  z.number()            →  number
bool IsActive           →  z.boolean()           →  boolean
```

### 2.4 Check Navigation Attributes
Verify OpenAPI extensions propagated:
- `[NavigationTarget]` → `x-navigation-target`
- `[Label]` → `x-label`
- `[HiddenColumn]` → `x-hidden-column`

## Phase 3: Update Documentation

### 3.1 Update coverage.md
- Update version numbers (API/UI commit hashes)
- Update coverage statistics
- Mark newly implemented features with ✅
- Update tables with new endpoints

### 3.2 Update comprehensive-gap-analysis.md
- Update statistics (controllers, repositories, services)
- Add "Recent Updates" section for new PRs
- Update feature tables (Dashboard, Reports, Exports, etc.)
- Update "Files Changed Summary" section

### 3.3 Create Implemented Feature Docs
For each new feature, create `docs/features/implemented/{feature}.md`:
```markdown
# Implemented: {Feature Name}

**Status**: Implemented
**PR**: #{number}
**Date**: {date}

## Summary
{Brief description}

## Endpoints Implemented
- `POST /api/{controller}/{action}`

## Models Created
| File | Purpose |
|------|---------|
| `{Model}.cs` | {purpose} |

## Frontend Integration
- API Client: `{client}.ts`
- Schema: `Z{Model}.ts`
- UI Page: `{page}/` (if applicable)

## Verification
{How to verify it works}
```

### 3.4 Clean Up Planned Features (CRITICAL)

**This step must not be skipped.**

1. **List all planned features**:
```bash
ls -la docs/features/planned/*.md docs/features/planned/api/*.md docs/features/planned/ui/*.md
```

2. **For each planned feature, check against PR changes**:
   - Was the feature implemented in this PR?
   - Was it partially implemented?
   - Are there any broken references?

3. **For fully completed features**:
   - Move file from `planned/` to `implemented/`
   - Update status in the file header
   - Update `planned/README.md` to remove reference
   - Add to `implemented/` section if not already there

4. **For partially completed features**:
   - Update status (e.g., "Backend Ready - Frontend Only Needed")
   - Document what was completed and what remains
   - Fix any broken cross-references

5. **Fix broken references** after file moves:
```bash
grep -r "planned/{moved-file}" docs/
grep -r "../frontend-grid-export" docs/  # Example: check for broken relative paths
```

6. **Verify planned/README.md** reflects current state:
   - Only lists features still pending
   - Links point to correct locations
   - "Recently Implemented" section updated

### 3.5 Clean Up All Cross-References
Search and update all cross-references:
```bash
grep -r "planned/{moved-file}" docs/
grep -r "frontend-grid-export" docs/  # Check all references to moved files
```

## Phase 4: Validate Documentation Structure

### 4.1 Check File Organization
```
docs/features/
├── README.md                  # Updated with current structure
├── coverage.md                # Updated statistics
├── comprehensive-gap-analysis.md
├── implemented/               # All completed features
│   └── {new-features}.md
├── planned/                   # Only remaining work
│   ├── api/                   # Backend-only planned
│   ├── ui/                    # Frontend-only planned
│   └── {full-stack}.md
└── decisions/                 # Confirmed decisions
```

### 4.2 Verify Links
Check all markdown links resolve:
- Internal doc references
- Cross-folder references
- External references

## Phase 5: Output Summary

### 5.1 Report to User
Summarize:
1. **Changes Reviewed**: List of commits/PRs analyzed
2. **New Implementations**: Features now available
3. **Documentation Updated**: Files modified
4. **Remaining Gaps**: What's still needed

### 5.2 Example Output Format
```markdown
## Branch Update Review Complete

### PRs Reviewed
- API PR #102: 4 new controllers, 6 new models
- UI PR #103: 3 new pages, API clients generated

### New Implementations
- ✅ ProductChangeHistoryController - IOC history queries
- ✅ ReplacedProductController - Product replacements
- ✅ AllergenDashboardController - Allergen stats
- ✅ QueryProductModel extensions (AllergenGroups, ContainsStatement)

### Documentation Updated
- coverage.md - Updated to 75% coverage
- comprehensive-gap-analysis.md - Added PR #102 section
- Created 8 new implemented/*.md files
- Moved 9 completed planned/*.md to implemented/

### Remaining Gaps
- Frontend grid export (backends ready, UI needed)
- Product approval workflow (P1)
```

## Phase 6: Checklist

Before completing review:

**Frontend/Backend Alignment:**
- [ ] All new controllers have frontend clients
- [ ] All model fields match between C# and Zod schemas
- [ ] Navigation attributes propagated to OpenAPI

**Documentation Updates:**
- [ ] coverage.md updated with new version numbers and statistics
- [ ] comprehensive-gap-analysis.md reflects current state
- [ ] CLAUDE.md migration status updated
- [ ] New implemented/*.md files created for each completed feature

**Planned Features Cleanup (CRITICAL):**
- [ ] Listed all planned/*.md files
- [ ] Checked each planned feature against PR changes
- [ ] Completed features moved to implemented/
- [ ] Partially complete features updated with current status
- [ ] All broken cross-references fixed
- [ ] planned/README.md reflects current state

**Structure Validation:**
- [ ] All markdown links resolve correctly
- [ ] README.md files reflect current structure
- [ ] No orphaned or duplicate documentation

## Usage Examples

### Example 1: Single Branch Update
```
User: I pulled latest into the API branch

Claude:
1. Check GreenOnion.API recent commits
2. Review new controllers/models
3. Verify frontend has matching clients (may note gaps)
4. Update backend-related documentation
5. Report findings
```

### Example 2: Both Branches Updated
```
User: Pulled in latest from both API and UI repos

Claude:
1. Check both submodules for recent commits
2. Cross-reference API changes with UI changes
3. Verify full frontend/backend alignment
4. Update all documentation
5. Report comprehensive findings
```

### Example 3: Specific PR Review
```
User: PR #102 and #103 were merged, review the changes

Claude:
1. Find commits associated with those PRs
2. Deep review of all changes in those PRs
3. Create detailed implementation docs
4. Update tracking documents
5. Report specific PR changes
```

## Notes

- Always verify data type mappings (especially DateTime → string in Zod)
- Check for nullable differences (C# `?` vs Zod `.optional()`)
- Navigation attributes are critical for UI generation
- Keep implemented/ and planned/ folders in sync
- Update coverage percentages based on actual counts
