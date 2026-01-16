# Protocol Import Workflow

**Version:** 1.3
**Last Updated:** 2026-01-16
**Purpose:** Import, classify, sanitize, and integrate new or updated protocols from the incoming folder into the protocol collection, with support for creating new categories as needed

---

## Overview

This workflow automates the process of reviewing, validating, and integrating protocols from a designated incoming folder (`./incoming/`) into the main protocol collection (`./protocols/`). It handles both new protocols and updates to existing ones, validates them against quality standards, sanitizes them to remove company/project-specific information, categorizes them appropriately, and updates the protocol registry. The result is a collection of generic, reusable protocols free of identifiable information.

---

## When to Use

- After adding new protocols to the `./incoming/` folder
- When updating existing protocols via the incoming folder
- Periodically to process accumulated protocol changes
- As part of a continuous protocol management routine

## Trigger Phrases

Use any of these phrases to initiate the protocol import workflow:

- `check incoming for updates`
- `I have new protocols`
- `import protocols`
- `process incoming protocols`
- `review incoming`

---

## Prerequisites

- The `./incoming/` folder exists and contains protocol files (markdown files)
- The `./protocols/` directory structure is established with base subdirectories: `documentation/`, `software/`, `workflows/`
- Access to `protocols/README.md` for registry updates
- Familiarity with `protocol-validation.md` standards
- Protocol files follow kebab-case naming convention
- Metadata is present in each incoming protocol (version, date, purpose)
- Ability to create new category folders if incoming protocols don't fit existing categories
- Understanding that new categories require: folder creation, category README.md, and registry updates

---

## Procedure

### Step 1: Scan Incoming Folder

Identify all markdown files in `./incoming/` that need processing.

**Actions:**
- List all `.md` files in `./incoming/`
- Create a working inventory of files to process
- Note the current count of incoming protocols
- Set aside any non-markdown files (invalid format)

**Documentation:**
- Record the number of new files
- Note any naming issues (spaces, caps, wrong format)

---

### Step 2: Classify Each Protocol

For each protocol file, determine its category and whether it's new or an update.

**Determine Category:**

Review the protocol content and metadata to identify the appropriate category:

| Category | Indicators | Examples |
|----------|-----------|----------|
| **documentation/** | Documentation standards, style guides, structure | documentation-style-guide, documentation-standards |
| **software/** | Analysis, validation, audits, code generation | security-audit, architectural-analysis, protocol-validation |
| **workflows/** | Development processes, lifecycle, task management | init-setup, start-work, change-review |

**Decision Logic:**
- Read the protocol's title and overview
- Match to category description
- If ambiguous, default to `software/` (for analysis/validation tools)
- If protocol doesn't fit existing categories, create a new category folder using kebab-case naming (e.g., `testing/`, `deployment/`, `security/`)
- Document new category creation in import report and update registry structure

**Creating New Categories:**
- Use lowercase folder name in kebab-case format
- Category must be generic and serve multiple protocols
- Add category to `protocols/README.md` Protocol Categories section
- Create a README.md in the new category folder with category description

**Identify Match Status:**

- **New Protocol:** Filename does not exist in any category folder
- **Update to Existing:** Filename matches a protocol in one of the category folders

**Document:**
- Protocol name
- Target category
- Status (new / update to {existing_protocol})
- Current version in incoming file

---

### Step 3: Validate Each Protocol

Before moving or integrating, validate using the protocol-validation standards.

**Execute Validation Checklist from protocol-validation.md:**

For each protocol, verify:

**Structure (Step 1):**
- [ ] All required sections present (title, metadata, overview, when to use, prerequisites, procedure, validation checklist, common issues, related protocols, change log)
- [ ] Logical section order
- [ ] Complete metadata (version, date, purpose)

**Content Quality (Step 2):**
- [ ] Clear, unambiguous instructions
- [ ] Technical terms defined
- [ ] Steps start with action verbs
- [ ] Edge cases addressed

**Style (Step 4):**
- [ ] Markdown renders correctly
- [ ] Proper formatting and hierarchy
- [ ] No typos or grammatical errors
- [ ] Follows documentation style guide

**Document Issues:**
- For each failed validation item, record the specific issue
- Create a list of required fixes before integration

**Conditional Validation Path:**

**If validation passes:** Continue to Step 4 (Sanitize for Reusability)

**If validation fails:**
1. Record all validation failures
2. Create a report documenting what needs to be fixed
3. Move the protocol to a `./incoming/rejected/` subfolder (create if needed)
4. Document the reason for rejection
5. Skip Steps 4-6 for this protocol
6. Continue to next protocol

---

### Step 4: Sanitize for Reusability

Ensure protocols are generic and project-agnostic by removing company names, internal project references, and identifiable information.

**Identify Sensitive Information:**

Scan the protocol for:

| Type | Examples | Replace With |
|------|----------|--------------|
| Company names | Acme Inc, TechCorp, internal company name | Remove or use generic "organization" |
| Project names | ProjectAlpha, Internal Portal, CodeName-X | {ProjectName} |
| Feature names | BlueFeature, ServiceY, internal codename | {FeatureName} |
| Component names | LegacyComponent, InternalAPI | {ComponentName} |
| Internal process names | Our deployment process, internal review | Generic equivalent |
| Internal team names | PlatformTeam, CoreServices | Generic role (e.g., "development team") |
| Specific product names | Product ABC, internal tools | Generic equivalent |
| Domain/server names | company.internal.com, internal-api | {InternalDomain}, {APIEndpoint} |
| Environment names | corp-staging, internal-prod | {Environment}, {StagingEnvironment} |

**Replacement Rules:**

1. **Company/Organization References:**
   - Remove vendor/company name when not essential
   - If essential to context, replace with "organization" or "company"
   - Example: "TechCorp's deployment process" → "the organization's deployment process"

2. **Project/Product Names:**
   - Replace with `{ProjectName}` placeholder
   - Example: "In ProjectAlpha, we..." → "In {ProjectName}, we..."

3. **Feature/Component Names:**
   - Replace with `{FeatureName}` or `{ComponentName}`
   - Example: "The BlueFeature module" → "The {FeatureName} module"

4. **Process Names:**
   - Generalize to describe the process type
   - Example: "Use our internal review process" → "Use a code review process"

5. **URLs/Domains:**
   - Replace with placeholders: `{InternalDomain}`, `{APIEndpoint}`, `{RepositoryURL}`
   - Example: "api.company.internal" → "{APIEndpoint}"

6. **File Paths with Internal References:**
   - Replace project-specific paths with generic structure
   - Example: "/teams/projectalpha/src" → "/{ProjectName}/src"

**Sanitization Process:**

1. Read the entire protocol content carefully
2. Create a list of all company/project-specific terms found
3. For each term, determine appropriate replacement
4. Replace all instances consistently throughout the protocol
5. Search for partial matches (e.g., "projectalpha" vs "ProjectAlpha")
6. Verify the protocol reads naturally with placeholders
7. Update any examples to use placeholders
8. Update code samples to use generic names instead of real internal names

**Validation After Sanitization:**

After sanitizing, verify:

- [ ] No company names remain
- [ ] No project codenames remain
- [ ] No internal product names remain
- [ ] No internal team references remain
- [ ] No internal domain names remain
- [ ] Generic placeholders used consistently: `{ProjectName}`, `{FeatureName}`, `{ComponentName}`, etc.
- [ ] Protocol reads naturally and is understandable with placeholders
- [ ] Examples and code samples are generic
- [ ] No accidental removal that breaks meaning
- [ ] Related protocols section does not reference removed information

**Conditional Sanitization Path:**

**If sanitization succeeds:** Continue to Step 5 (Handle Version Updates)

**If sanitization cannot be completed:**
1. Record the sanitization issues
2. Move protocol to `./incoming/rejected/` subfolder
3. Document the reason: "Cannot be made generic: {specific issue}"
4. Example: "Cannot be made generic: heavily references internal product names that are essential context"
5. Skip Steps 5-6 for this protocol
6. Continue to next protocol

---

### Step 5: Handle Version Updates

For updates to existing protocols, manage version increments properly.

**For NEW protocols:**
- [ ] Version should start at 1.0
- [ ] Last Updated date should be today
- [ ] No prior change log entries (only creation entry)

**For UPDATES to existing protocols:**

1. **Retrieve current version** from the existing protocol in `./protocols/{category}/{filename}`
2. **Compare versions:**
   - If incoming version ≤ current version: This is a REJECTED update (version conflict)
     - Move to `./incoming/rejected/` with note "Version conflict: incoming v{X.Y} vs existing v{Y.Z}"
     - Skip remaining steps for this protocol
   - If incoming version > current version: Continue with upgrade

3. **Verify version increment is appropriate:**
   - Major change (workflow changes): X.0 (e.g., 1.0 → 2.0)
   - Minor change (new sections, clarifications): X.Y (e.g., 1.0 → 1.1)
   - Use protocol-validation.md Step 6 as reference

4. **Validate version number format:**
   - [ ] Semantic versioning (e.g., 1.0, 1.1, 2.0)
   - [ ] Not pre-release versions (no alpha, beta, rc)
   - [ ] Single increment only (not jumping 1.0 to 3.0)

---

### Step 6: Move Protocols to Target Category

Move validated protocols from `./incoming/` to the appropriate category folder.

**For NEW protocols:**
1. Move file from `./incoming/{filename}` to `./protocols/{category}/{filename}`
2. Verify file exists in target location
3. Delete from `./incoming/`

**For UPDATED protocols:**
1. Back up existing protocol: Copy `./protocols/{category}/{filename}` to `./protocols/{category}/.backups/{filename}.v{old-version}.bak`
2. Replace protocol: Move incoming file to `./protocols/{category}/{filename}`, overwriting existing
3. Verify replacement is complete
4. Delete from `./incoming/`

**Validation:**
- [ ] File exists in target category folder
- [ ] File no longer exists in `./incoming/`
- [ ] File is readable and properly formatted

---

### Step 7: Update Protocol Registry

Update `./protocols/README.md` to reflect new and updated protocols.

**For NEW protocols:**
1. Add entry to appropriate category table (Documentation, Software Analysis, or Workflow)
2. Format: `| [protocol-name.md] | X.Y | Description of purpose |`
3. Maintain alphabetical order within category
4. Add markdown link reference at bottom of file in Links section
5. Update the Active Protocols section count if present

**For UPDATED protocols:**
1. Find protocol in appropriate category table
2. Update version number to new version
3. Verify description is still accurate (update if needed)
4. No change to link references required

**Registry Checklist:**
- [ ] Protocol listed in correct category table
- [ ] Version number matches the protocol file
- [ ] Description is clear and accurate
- [ ] Markdown links are added/updated at bottom
- [ ] Tables maintain proper formatting (aligned pipes)
- [ ] Alphabetical order maintained
- [ ] Version History section updated with date and changes
- [ ] README renders correctly in markdown

---

### Step 8: Update Related Protocol References

Verify and update any cross-references in related protocols.

**Action Items:**
1. Check if new protocol is related to existing protocols
2. Update "Related Protocols" sections in:
   - The new/updated protocol
   - Any existing protocols that now relate to it
3. Verify all internal links in related sections work correctly
4. Update dependency documentation if workflow order changed

**Validation:**
- [ ] All referenced protocols exist
- [ ] Links point to correct files
- [ ] Bidirectional relationships documented
- [ ] No circular dependencies introduced

---

### Step 9: Archive and Report

Complete the import process and document results.

**Clean Up:**
1. Verify `./incoming/` folder is empty (except `./incoming/rejected/` if used)
2. Archive `./incoming/rejected/` contents with a date-stamped backup if any rejections occurred

**Create Import Report:**
- Total files processed
- New protocols added (count, names, categories)
- Existing protocols updated (count, names, version changes)
- Rejected protocols (count, reasons)
- New categories created (if any)
- Registry updates made
- Any warnings or issues noted

**Documentation:**
- Update any master changelog if maintained at project level
- Record the import completion date and summary

---

## Validation Checklist

**Incoming Folder:**
- [ ] All incoming files scanned
- [ ] Files properly named (kebab-case)
- [ ] All markdown files identified

**Classification:**
- [ ] Each protocol assigned to correct category
- [ ] New vs. update status determined for all files
- [ ] No protocols unclassified
- [ ] New categories identified (if needed) and documented
- [ ] New category folders created with appropriate README.md files

**Validation:**
- [ ] All protocols pass protocol-validation standards
- [ ] Failed validations documented and filed
- [ ] No invalid protocols moved to main collection

**Sanitization:**
- [ ] All company names removed or generalized
- [ ] All project codenames replaced with {ProjectName}
- [ ] All internal product names generalized
- [ ] All team names replaced with generic roles
- [ ] All domain/server names replaced with placeholders
- [ ] All environment names generalized
- [ ] Examples and code samples are generic
- [ ] Protocol reads naturally with placeholders
- [ ] No identifiable information remains
- [ ] Non-sanitizable protocols rejected to incoming/rejected/

**Version Management:**
- [ ] New protocols start at v1.0
- [ ] Updates have higher version than existing
- [ ] Version increments are appropriate
- [ ] Version conflicts identified and rejected

**File Movement:**
- [ ] All valid protocols moved to target folders
- [ ] Incoming folder cleaned of processed files
- [ ] Backups created for updates
- [ ] No duplicate files left behind

**Registry Updates:**
- [ ] README.md updated with all new protocols
- [ ] Versions updated for all modified protocols
- [ ] Descriptions accurate and current
- [ ] Links added/updated
- [ ] Formatting correct (aligned tables)
- [ ] Alphabetical order maintained

**Related References:**
- [ ] Related protocols updated
- [ ] Cross-references verified
- [ ] No broken links introduced

**Final State:**
- [ ] `./incoming/` folder clean
- [ ] All files in correct locations
- [ ] Registry accurate and complete
- [ ] Import report generated
- [ ] All protocols are generic and reusable

---

## Common Issues

| Issue | Cause | Resolution |
|-------|-------|----------|
| Files already in incoming folder unclear | Previous import incomplete | Review incoming folder contents, determine if old/new, move or delete as appropriate |
| Protocol fails validation | Missing sections or poor content | Reject to incoming/rejected/, document required fixes, return to contributor |
| Contains company/project names | Identifiable information not removed | Reject to incoming/rejected/, request sanitization of all company/project references |
| Contains internal domain names | Server/environment references not generalized | Reject to incoming/rejected/, request replacement with placeholders like {APIEndpoint}, {InternalDomain} |
| Cannot sanitize without losing meaning | Protocol heavily relies on specific internal context | Reject with note "Cannot be made generic: core content is organization-specific" |
| Inconsistent placeholder usage | Placeholders used but not consistently | Reject with note "Inconsistent sanitization: {ProjectName} used in some sections, specific name in others" |
| Version conflict on update | Incoming version same or lower than existing | Reject update, request new version number higher than current |
| Wrong category assigned | Misunderstood protocol purpose | Review protocol content carefully against category definitions, reassign if needed |
| Broken links after update | Protocol references other protocols that changed | Search for references, update paths and link names accordingly |
| File already exists in target | Duplicate protocol | Reject with note "Duplicate of existing protocol" |
| Metadata missing | Incomplete protocol file | Reject to incoming/rejected/, request metadata completion |
| Filename doesn't match kebab-case | Naming convention violation | Reject with note "Filename must use kebab-case" (e.g., my-protocol.md not MyProtocol.md) |

---

## Related Protocols

- [Protocol Validation](../software/protocol-validation.md) - Quality assurance for protocols
- [Documentation Standards](../documentation/documentation-standards.md) - Naming and file organization conventions
- [Documentation Style Guide](../documentation/documentation-style-guide.md) - Content standards
- [Comprehensive Analysis Checklist](../software/comprehensive-analysis-checklist.md) - Completeness verification

---

## Change Log

- 2026-01-16 v1.3: Added trigger phrases for easy execution
- 2026-01-16 v1.2: Added support for creating new protocol categories and improved category documentation
- 2026-01-16 v1.1: Added sanitization step to remove company/project-specific information and ensure protocols are generic and reusable
- 2026-01-16 v1.0: Initial protocol import workflow with support for new and updated protocols
