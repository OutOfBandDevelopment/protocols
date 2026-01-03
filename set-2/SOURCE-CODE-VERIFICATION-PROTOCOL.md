# Source Code Verification Protocol

**Purpose**: Standardized protocol for verifying documentation accuracy against actual source code.

**Version**: 1.0
**Last Updated**: 2026-01-03
**Target**: All technical documentation that references source code

---

## Overview

This protocol provides a systematic approach to:
1. **Verify documentation** against actual source code
2. **Identify documentation errors** (incorrect, outdated, missing)
3. **Correct documentation** to match source code
4. **Prevent future drift** between docs and code
5. **Build confidence** in documentation accuracy

**Key Concept**: Documentation can describe a system that doesn't exist, or exist but be wrong. This protocol ensures docs match reality (the source code).

---

## When to Use This Protocol

Use this protocol when:
- ✅ After creating/updating architecture documentation
- ✅ After creating/updating API documentation
- ✅ After creating/updating data model documentation
- ✅ When documentation accuracy is questioned
- ✅ Before important presentations/audits
- ✅ Periodic documentation quality reviews (quarterly)
- ✅ After discovering documentation error

Skip this protocol when:
- ❌ Documentation just created and verified (no time passed)
- ❌ No code changes since last verification
- ❌ Documentation is user-facing only (no code references)

---

## Prerequisites

Before starting, verify:
- [ ] Documentation to verify exists
- [ ] Access to complete source code
- [ ] Understanding of which docs need verification
- [ ] Time allocated (1-8 hours depending on scope)
- [ ] Ability to search codebase effectively

---

## Phase 1: Scope Verification Work

### 1.1 Identify Documents to Verify

**Purpose**: Determine what needs verification

**Document Types Requiring Verification**:

| Document Type | Verification Need | Priority |
|---------------|-------------------|----------|
| **Architecture Docs** | HIGH - Contains code references, entity names, API names | CRITICAL |
| **API Documentation** | CRITICAL - Must match actual signatures | CRITICAL |
| **Data Model Docs** | CRITICAL - Must match actual schema | CRITICAL |
| **Security Docs** | HIGH - Must match actual implementation | HIGH |
| **Configuration Docs** | HIGH - Must match actual config keys/values | HIGH |
| **User Guides** | MEDIUM - Should match UI behavior | MEDIUM |
| **Deployment Docs** | MEDIUM - Should match actual deployment | MEDIUM |
| **Release Notes** | LOW - Historical, less critical | LOW |

### 1.2 Create Verification Plan

Create `.verification/verification-plan.md`:

```markdown
# Source Code Verification Plan

**Verification Date**: [YYYY-MM-DD]
**Scope**: [Full docs / Specific docs / Recent changes]
**Estimated Effort**: [X] hours
**Assignee**: [Name]

---

## Documents to Verify

| Document | Priority | Verification Type | Effort | Status |
|----------|----------|-------------------|--------|--------|
| `05-data-model.md` | CRITICAL | Full entity verification | 3h | 🔴 Pending |
| `04-apis.md` | CRITICAL | API signature verification | 2h | 🔴 Pending |
| `06-authentication-security.md` | HIGH | Implementation verification | 2h | 🔴 Pending |
| `08-configuration-management.md` | HIGH | Config key verification | 1h | 🔴 Pending |

**Total Effort**: 8 hours

---

## Verification Criteria

For each document, verify:
- [ ] **Entity names** match code
- [ ] **Property names/types** match code
- [ ] **Method signatures** match code
- [ ] **Relationships** match code
- [ ] **Configuration keys** match code
- [ ] **File/line references** are correct
- [ ] **Code examples** compile and run
- [ ] **Descriptions** match actual behavior

---

## Risk Assessment

**High Risk Areas** (likely to have errors):
- Recently updated docs (may have copy-paste errors)
- Old docs (code may have changed)
- Complex entities with many properties

**Low Risk Areas**:
- Recently verified docs
- Simple entities
- Stable code areas
```

---

## Phase 2: Verify Entity/Table Documentation

### 2.1 Entity Name Verification

**Purpose**: Ensure entity names in docs match code

**Process**:

1. **Extract Entity Names from Docs**:
   ```bash
   # Extract entity names from data model doc
   grep "^### " docs/design/05-data-model.md | sed 's/### //; s/ Table//; s/ Entity//' | sort > /tmp/doc-entities.txt

   # Show entities
   cat /tmp/doc-entities.txt
   ```

2. **Extract Entity Names from Code**:
   ```bash
   # Find entity classes
   find ./src/ -path "*/Database/Tables/*.cs" -o -path "*/Entities/*.cs" | sed 's|.*/||; s|\.cs||' | sort > /tmp/code-entities.txt

   # Or use grep for entity classes
   grep -r "public class.*Entity" --include="*.cs" ./src/ | sed 's/.*public class //; s/ .*//' | sort | uniq > /tmp/code-entities.txt
   ```

3. **Compare**:
   ```bash
   # Find entities in docs but not in code (doc errors)
   comm -23 /tmp/doc-entities.txt /tmp/code-entities.txt > .verification/doc-only-entities.txt

   # Find entities in code but not in docs (doc gaps)
   comm -13 /tmp/doc-entities.txt /tmp/code-entities.txt > .verification/code-only-entities.txt

   # Show mismatches
   echo "Entities in docs but not in code (ERRORS):"
   cat .verification/doc-only-entities.txt

   echo "Entities in code but not in docs (GAPS):"
   cat .verification/code-only-entities.txt
   ```

4. **Record Findings**:

   Create `.verification/entity-name-verification.md`:

   ```markdown
   # Entity Name Verification

   **Verification Date**: [YYYY-MM-DD]
   **Verified Against**: Commit [hash]

   ---

   ## Verification Results

   **Total Entities in Docs**: [count]
   **Total Entities in Code**: [count]
   **Matches**: [count]
   **Mismatches**: [count]
   **Accuracy**: [percentage]%

   ---

   ## Errors Found (In Docs but Not in Code)

   ### StreamActivity
   **Documented In**: `docs/design/05-data-model.md:145`
   **Issue**: Entity name incorrect - should be "OpenStreamActivity"
   **Code Location**: `/current/src/Source/Database/Tables/OpenStreamActivity.cs:1`
   **Severity**: CRITICAL
   **Fix**: Update doc to use "OpenStreamActivity"

   [Continue for all errors...]

   ---

   ## Gaps Found (In Code but Not in Docs)

   ### AuditLog
   **Code Location**: `/current/src/Source/Database/Tables/AuditLog.cs:1`
   **Issue**: Entity exists in code but not documented
   **Severity**: HIGH
   **Fix**: Add AuditLog to data model doc

   [Continue for all gaps...]
   ```

### 2.2 Property/Column Verification

**Purpose**: Ensure documented properties match code

**Process**:

For each entity, verify properties:

1. **Read Documentation**:
   ```markdown
   ### Patient Table

   | Column | Type | Nullable | Description |
   |--------|------|----------|-------------|
   | Id | GUID | No | Primary key |
   | FirstName | VARCHAR(100) | No | Patient first name |
   | LastName | VARCHAR(100) | No | Patient last name |
   | DateOfBirth | Date | Yes | Patient date of birth |
   ```

2. **Read Source Code**:
   ```bash
   code ./src/Database/Tables/Patient.cs
   ```

   ```csharp
   public class Patient
   {
       public Guid Id { get; set; }
       public string FirstName { get; set; }  // max length 100
       public string LastName { get; set; }   // max length 100
       public DateTime? DateOfBirth { get; set; }
       public string MiddleName { get; set; }  // NOT in docs!
   }
   ```

3. **Compare**:
   - ✅ Id: Matches
   - ✅ FirstName: Matches (type, nullable)
   - ✅ LastName: Matches
   - ✅ DateOfBirth: Matches (type, nullable)
   - ❌ MiddleName: MISSING from docs

4. **Record Findings**:

   Create `.verification/property-verification.md`:

   ```markdown
   # Property Verification - Patient Entity

   **Entity**: Patient
   **Documentation**: `docs/design/05-data-model.md:87`
   **Source Code**: `/current/src/Source/Database/Tables/Patient.cs:15`
   **Verification Date**: [YYYY-MM-DD]

   ---

   ## Verification Results

   | Property | In Docs | In Code | Match | Type Match | Nullable Match |
   |----------|---------|---------|-------|------------|----------------|
   | Id | ✅ | ✅ | ✅ | ✅ GUID | ✅ No |
   | FirstName | ✅ | ✅ | ✅ | ✅ VARCHAR(100) | ✅ No |
   | LastName | ✅ | ✅ | ✅ | ✅ VARCHAR(100) | ✅ No |
   | DateOfBirth | ✅ | ✅ | ✅ | ✅ Date | ✅ Yes |
   | MiddleName | ❌ | ✅ | ❌ | - | - |

   **Accuracy**: 80% (4/5 properties)

   ---

   ## Errors Found

   ### Missing Property: MiddleName
   **Severity**: MEDIUM
   **Issue**: Property exists in code but not in docs
   **Code**: `public string MiddleName { get; set; }`
   **Code Location**: `/current/src/Source/Database/Tables/Patient.cs:20`
   **Fix**: Add MiddleName property to documentation

   ---

   ## Recommended Documentation Update

   ```markdown
   ### Patient Table

   | Column | Type | Nullable | Description |
   |--------|------|----------|-------------|
   | Id | GUID | No | Primary key |
   | FirstName | VARCHAR(100) | No | Patient first name |
   | MiddleName | VARCHAR(100) | Yes | Patient middle name |  <!-- ADD -->
   | LastName | VARCHAR(100) | No | Patient last name |
   | DateOfBirth | Date | Yes | Patient date of birth |
   ```
   ```

### 2.3 Relationship Verification

**Purpose**: Ensure documented relationships match code

**Process**:

1. **Read Documented Relationships**:
   ```markdown
   **Relationships**:
   - HasMany: Studies (via Patient.Id)
   - HasMany: ForeignFiles (via Patient.Id)
   - BelongsTo: Facility (via FacilityId)
   ```

2. **Check Source Code**:
   ```bash
   # Find navigation properties
   grep -A 5 "public class Patient" ./src/Database/Tables/Patient.cs
   ```

   ```csharp
   public class Patient
   {
       // ... properties ...

       // Navigation properties
       public virtual ICollection<Study> Studies { get; set; }
       public virtual ICollection<ForeignFile> ForeignFiles { get; set; }
       public virtual Facility Facility { get; set; }
       public Guid FacilityId { get; set; }

       // MISSING from docs:
       public virtual ICollection<OpenStreamActivity> StreamActivities { get; set; }
   }
   ```

3. **Verify Foreign Keys**:
   ```csharp
   // In Study class
   public Guid PatientId { get; set; }
   public virtual Patient Patient { get; set; }
   ```

4. **Record Findings**:
   ```markdown
   ## Relationship Verification - Patient Entity

   | Relationship | In Docs | In Code | FK Correct | Status |
   |--------------|---------|---------|------------|--------|
   | HasMany Studies | ✅ | ✅ | ✅ PatientId | ✅ Match |
   | HasMany ForeignFiles | ✅ | ✅ | ✅ PatientId | ✅ Match |
   | BelongsTo Facility | ✅ | ✅ | ✅ FacilityId | ✅ Match |
   | HasMany StreamActivities | ❌ | ✅ | ✅ PatientId | ❌ MISSING |

   **Accuracy**: 75% (3/4 relationships)

   **ERROR**: Missing relationship documentation for StreamActivities
   ```

---

## Phase 3: Verify API Documentation

### 3.1 API Signature Verification

**Purpose**: Ensure documented API signatures match code

**Process**:

1. **Read Documented API**:
   ```markdown
   ### RecordStreamOpen

   **Signature**:
   ```csharp
   Task<Guid> RecordStreamOpen(Guid patientId, string streamType);
   ```

   **Parameters**:
   - `patientId` (Guid): ID of the patient
   - `streamType` (string): Type of stream (e.g., "EEG", "EMG")

   **Returns**: `Guid` - Activity ID
   ```

2. **Locate Source Code**:
   ```bash
   # Find the method
   grep -r "RecordStreamOpen" --include="*.cs" ./src/

   # Or use more specific search
   grep -A 10 "RecordStreamOpen" ./src/Services/StreamTrackingService.cs
   ```

3. **Compare Signatures**:
   ```csharp
   // Actual code
   public async Task<Guid> RecordStreamOpen(
       Guid patientId,
       string streamType,
       string machineName)  // MISSING from docs!
   {
       // implementation
   }
   ```

4. **Record Findings**:

   Create `.verification/api-verification.md`:

   ```markdown
   # API Signature Verification

   **API**: StreamTrackingService.RecordStreamOpen
   **Documentation**: `docs/api/stream-tracking.md:45`
   **Source Code**: `/current/src/Source/Services/StreamTrackingService.cs:67`
   **Verification Date**: [YYYY-MM-DD]

   ---

   ## Signature Comparison

   **Documented**:
   ```csharp
   Task<Guid> RecordStreamOpen(Guid patientId, string streamType);
   ```

   **Actual**:
   ```csharp
   Task<Guid> RecordStreamOpen(Guid patientId, string streamType, string machineName);
   ```

   **Match**: ❌ NO - Parameter count mismatch

   ---

   ## Errors Found

   ### Missing Parameter: machineName
   **Severity**: CRITICAL
   **Issue**: Documentation missing third parameter
   **Parameter**: `string machineName` - Name of client machine
   **Fix**: Add machineName parameter to documentation

   ---

   ## Corrected Documentation

   ```markdown
   ### RecordStreamOpen

   **Signature**:
   ```csharp
   Task<Guid> RecordStreamOpen(Guid patientId, string streamType, string machineName);
   ```

   **Parameters**:
   - `patientId` (Guid): ID of the patient
   - `streamType` (string): Type of stream (e.g., "EEG", "EMG")
   - `machineName` (string): Name of the client machine recording the stream

   **Returns**: `Guid` - Activity ID for the opened stream
   ```
   ```

### 3.2 Return Type Verification

**Purpose**: Ensure documented return types match code

**Process**:

1. **Check Documented Return**:
   ```markdown
   **Returns**: `Guid` - Activity ID
   ```

2. **Check Actual Return**:
   ```csharp
   public async Task<Guid> RecordStreamOpen(...)
   {
       var activity = new OpenStreamActivity { ... };
       await repository.InsertAsync(activity);
       return activity.Id;  // Returns Guid ✅
   }
   ```

3. **Verify**:
   - ✅ Return type: `Guid` matches
   - ✅ Description: "Activity ID" is accurate

### 3.3 Exception Documentation Verification

**Purpose**: Ensure documented exceptions match code

**Process**:

1. **Read Documented Exceptions**:
   ```markdown
   **Exceptions**:
   - `ArgumentException`: If patientId is invalid
   - `UnauthorizedException`: If user not authorized
   ```

2. **Check Source Code**:
   ```csharp
   public async Task<Guid> RecordStreamOpen(...)
   {
       if (patientId == Guid.Empty)
           throw new ArgumentException("Invalid patient ID", nameof(patientId));

       var patient = await patientRepository.GetByIdAsync(patientId);
       if (patient == null)
           throw new NotFoundException("Patient not found");  // MISSING from docs!

       if (!authService.CanAccessPatient(currentUser, patientId))
           throw new UnauthorizedException("User cannot access patient");

       // ...
   }
   ```

3. **Record Findings**:
   ```markdown
   ## Exception Verification

   | Exception | In Docs | In Code | Condition Match |
   |-----------|---------|---------|-----------------|
   | ArgumentException | ✅ | ✅ | ✅ Invalid patientId |
   | UnauthorizedException | ✅ | ✅ | ✅ Not authorized |
   | NotFoundException | ❌ | ✅ | - Patient not found |

   **Accuracy**: 67% (2/3 exceptions)

   **ERROR**: Missing NotFoundException documentation
   ```

---

## Phase 4: Verify Configuration Documentation

### 4.1 Configuration Key Verification

**Purpose**: Ensure documented config keys match code

**Process**:

1. **Extract Keys from Docs**:
   ```bash
   # Extract config keys from docs
   grep "Key\`:" docs/design/08-configuration-management.md | sed 's/.*`\(.*\)`.*/\1/' | sort > /tmp/doc-keys.txt
   ```

2. **Extract Keys from Code**:
   ```bash
   # Find config key references in code
   grep -r "ConfigurationManager.AppSettings\[" --include="*.cs" ./src/ | sed 's/.*\["\(.*\)"\].*/\1/' | sort | uniq > /tmp/code-keys.txt

   # Or search for key string literals
   grep -r "\"Features\." --include="*.cs" ./src/ | grep -oP '"Features\.[^"]*"' | tr -d '"' | sort | uniq
   ```

3. **Compare**:
   ```bash
   # Keys in docs but not in code
   comm -23 /tmp/doc-keys.txt /tmp/code-keys.txt > .verification/doc-only-keys.txt

   # Keys in code but not in docs
   comm -13 /tmp/doc-keys.txt /tmp/code-keys.txt > .verification/code-only-keys.txt
   ```

4. **Verify Default Values**:
   ```csharp
   // Check actual default value
   var enableTracking = ConfigurationManager.AppSettings["Features.EnableStreamTracking"] ?? "true";  // Default: "true" ✅
   ```

5. **Record Findings**:
   ```markdown
   # Configuration Key Verification

   **Config Key**: `Features.EnableStreamTracking`
   **Documentation**: `docs/design/08-configuration-management.md:234`
   **Source Code**: `/current/src/Source/Configuration/FeatureFlags.cs:12`

   **Documented Default**: `true`
   **Actual Default**: `true`
   **Match**: ✅ YES

   **Documented Type**: Boolean
   **Actual Type**: Boolean (parsed from string)
   **Match**: ✅ YES
   ```

---

## Phase 5: Verify Code Examples

### 5.1 Extract Code Examples from Docs

**Purpose**: Ensure code examples compile and run

**Process**:

1. **Find Code Examples**:
   ```bash
   # Find all code blocks in docs
   grep -A 10 '```csharp' docs/design/*.md > .verification/code-examples.txt
   ```

2. **For Each Example**:

   **Documented Example**:
   ````markdown
   **Example**:
   ```csharp
   var activityId = await streamTrackingService.RecordStreamOpen(
       patientId: Guid.Parse("12345678-1234-1234-1234-123456789012"),
       streamType: "EEG"
   );
   ```
   ````

3. **Create Test File**:
   ```csharp
   // /tmp/test-example.cs
   using System;
   using System.Threading.Tasks;

   public class ExampleTest
   {
       public async Task TestExample()
       {
           var streamTrackingService = GetStreamTrackingService();
           var activityId = await streamTrackingService.RecordStreamOpen(
               patientId: Guid.Parse("12345678-1234-1234-1234-123456789012"),
               streamType: "EEG"  // MISSING: machineName parameter!
           );
       }
   }
   ```

4. **Try to Compile**:
   ```bash
   csc /tmp/test-example.cs /reference:StreamTrackingService.dll
   # ERROR: Missing required parameter 'machineName'
   ```

5. **Record Findings**:
   ```markdown
   # Code Example Verification

   **Example**: RecordStreamOpen usage
   **Documentation**: `docs/api/stream-tracking.md:67`
   **Status**: ❌ COMPILATION ERROR

   **Error**:
   ```
   CS7036: There is no argument given that corresponds to the required formal parameter 'machineName'
   ```

   **Issue**: Example missing required parameter
   **Severity**: CRITICAL - Users will copy broken code

   **Fix**: Update example to include machineName
   ```

---

## Phase 6: Create Verification Report

### 6.1 Aggregate Findings

**Purpose**: Summarize all verification findings

Create `.verification/verification-report.md`:

```markdown
# Source Code Verification Report

**Verification Date**: [YYYY-MM-DD]
**Verified Against**: Commit [hash]
**Scope**: [Full documentation / Specific docs]
**Verifier**: [Name]

---

## Executive Summary

**Documents Verified**: [count]
**Verification Duration**: [X] hours
**Overall Accuracy**: [percentage]%

**Issues Found**: [count]
- CRITICAL: [count]
- HIGH: [count]
- MEDIUM: [count]
- LOW: [count]

**Recommendation**: [PASS / NEEDS CORRECTION / MAJOR REVISION NEEDED]

---

## Verification Results by Document

### docs/design/05-data-model.md

**Status**: ⚠️ NEEDS CORRECTION
**Accuracy**: 85%
**Issues**: 12 errors found

**Critical Errors**:
- Entity name mismatch: "StreamActivity" should be "OpenStreamActivity"
- Missing entity: MiddleName property not documented
- Missing relationship: HasMany StreamActivities not documented

**High Errors**:
- Incorrect type: DateOfBirth type mismatch (DateTime vs Date)

**Medium Errors**:
- Missing index documentation for IX_Patient_LastName

**Verification Details**: `.verification/data-model-verification.md`

### docs/design/04-apis.md

**Status**: ❌ MAJOR REVISION NEEDED
**Accuracy**: 60%
**Issues**: 8 errors found

**Critical Errors**:
- RecordStreamOpen signature incorrect (missing machineName parameter)
- Code example doesn't compile (missing parameter)
- Missing exception: NotFoundException not documented

**High Errors**:
- GetStreamHistory return type incorrect

**Verification Details**: `.verification/api-verification.md`

### docs/design/06-authentication-security.md

**Status**: ✅ VERIFIED
**Accuracy**: 100%
**Issues**: 0 errors found

**Notes**:
- All security mechanisms verified against source
- Password hashing algorithm correctly documented
- Session management correctly documented

**Verification Details**: `.verification/security-verification.md`

[Continue for all documents...]

---

## Summary by Issue Type

### Entity/Table Documentation

**Total Entities Verified**: 32
**Matches**: 28
**Errors**: 4
**Accuracy**: 88%

**Errors**:
1. StreamActivity → OpenStreamActivity (name mismatch)
2. Missing entity: MiddleName property
3. Missing relationship: StreamActivities
4. Incorrect type: DateOfBirth

### API Documentation

**Total APIs Verified**: 15
**Matches**: 9
**Errors**: 6
**Accuracy**: 60%

**Errors**:
1. RecordStreamOpen signature (missing parameter)
2. RecordStreamClose signature (wrong return type)
3. GetStreamHistory return type (incorrect)
4. Missing exception: NotFoundException (3 APIs)

### Configuration Documentation

**Total Keys Verified**: 25
**Matches**: 24
**Errors**: 1
**Accuracy**: 96%

**Errors**:
1. StreamTracking.MaxActivitiesPerDay (undocumented key)

### Code Examples

**Total Examples Verified**: 8
**Compile**: 5
**Fail**: 3
**Accuracy**: 63%

**Errors**:
1. RecordStreamOpen example (missing parameter)
2. CreatePatient example (wrong namespace)
3. Configuration example (incorrect key name)

---

## Critical Issues Requiring Immediate Fix

### 1. API Signature Mismatches (CRITICAL)
**Count**: 3
**Impact**: Developers will use incorrect signatures, causing runtime errors
**Fix Effort**: 2 hours
**Priority**: P0 - Fix today

**Issues**:
- RecordStreamOpen missing machineName parameter
- RecordStreamClose wrong return type
- GetStreamHistory wrong return type

**Recommendation**: Update docs immediately, add note about version where signature changed

### 2. Broken Code Examples (CRITICAL)
**Count**: 3
**Impact**: Users will copy broken code, causing compilation errors
**Fix Effort**: 1 hour
**Priority**: P0 - Fix today

**Issues**:
- RecordStreamOpen example doesn't compile
- CreatePatient example wrong namespace
- Configuration example incorrect key

**Recommendation**: Test all examples before publishing

### 3. Entity Name Mismatches (HIGH)
**Count**: 1
**Impact**: Confusion, searching for wrong class names
**Fix Effort**: 30 minutes
**Priority**: P1 - Fix this week

**Issues**:
- StreamActivity should be OpenStreamActivity

**Recommendation**: Global search/replace, verify all references

---

## Documentation Corrections Summary

**Total Corrections Required**: 22
**Estimated Effort**: 8 hours
**Priority Breakdown**:
- P0 (CRITICAL): 6 issues - 3 hours
- P1 (HIGH): 8 issues - 3 hours
- P2 (MEDIUM): 6 issues - 1.5 hours
- P3 (LOW): 2 issues - 0.5 hours

---

## Recommendations

### Immediate Actions
1. Fix all CRITICAL issues (API signatures, code examples) - 3 hours
2. Update entity name mismatch - 30 minutes
3. Test all code examples - 1 hour

### Short-Term Actions (This Week)
1. Fix all HIGH priority issues - 3 hours
2. Add missing entity documentation - 2 hours
3. Update relationship documentation - 1 hour

### Long-Term Actions
1. Implement automated code example testing
2. Add CI check for documentation/code consistency
3. Schedule quarterly verification reviews
4. Create source code reference automation

### Process Improvements
1. **Require verification** before merging documentation PRs
2. **Automate signature extraction** from source code
3. **Test code examples** in CI/CD pipeline
4. **Link docs to source** with automation (like JSDoc/Doxygen)

---

## Next Steps

- [ ] Create GitHub issues for each CRITICAL error
- [ ] Assign owners for corrections
- [ ] Schedule corrections review meeting
- [ ] Update documentation
- [ ] Re-verify after corrections
- [ ] Update CLAUDE.md with verification session

---

**Report Completed**: [YYYY-MM-DD]
**Next Verification**: [date + 3 months]
**Verifier**: [Name]
```

---

## Phase 7: Correct Documentation Errors

### 7.1 Fix Critical Errors

**Purpose**: Immediately fix critical documentation errors

**For Each Critical Error**:

1. **Open Documentation**:
   ```bash
   code docs/api/stream-tracking.md
   ```

2. **Locate Error**:
   - Find the incorrect section
   - Review verification report for correct information

3. **Correct Error**:
   - Update signature
   - Update parameters
   - Update return type
   - Update exceptions
   - Update code example

4. **Add Verification Note**:
   ```markdown
   **Source Verification**: ✅ Verified against `/current/src/Source/Services/StreamTrackingService.cs:67` on 2026-01-03
   ```

5. **Test Code Example** (if applicable):
   ```bash
   # Extract example to file
   # Compile
   # Verify it works
   ```

6. **Commit Fix**:
   ```bash
   git add docs/api/stream-tracking.md
   git commit -m "docs: fix RecordStreamOpen API signature (critical)

   CRITICAL FIX: Updated RecordStreamOpen signature to include missing machineName parameter

   Issue: Documentation missing third parameter
   Source: /current/src/Source/Services/StreamTrackingService.cs:67
   Verified: 2026-01-03

   Also updated:
   - Parameters section
   - Code example (now compiles)
   - Exception documentation

   Closes: SOURCE-VERIFICATION finding #1"
   ```

### 7.2 Fix High Priority Errors

**Purpose**: Fix high priority errors in next sprint

**Process**: Same as critical, but on schedule (not immediate)

### 7.3 Track Corrections

Create `.verification/corrections-tracker.md`:

```markdown
# Verification Corrections Tracker

## Critical Errors

- [x] RecordStreamOpen signature - Fixed 2026-01-03 (commit abc123)
- [x] RecordStreamOpen example - Fixed 2026-01-03 (commit abc123)
- [x] RecordStreamClose return type - Fixed 2026-01-03 (commit def456)
- [ ] GetStreamHistory return type - Assigned to [name], due 2026-01-04

## High Priority Errors

- [ ] StreamActivity name mismatch - Assigned to [name], due 2026-01-05
- [ ] Missing MiddleName property - Assigned to [name], due 2026-01-05

[Continue...]
```

---

## Success Metrics

This protocol is successful when:

- ✅ **All Entities Verified**: Entity names, properties, relationships match code
- ✅ **All APIs Verified**: Signatures, parameters, return types, exceptions match code
- ✅ **All Config Verified**: Keys, defaults, types match code
- ✅ **All Examples Tested**: Code examples compile and run
- ✅ **Errors Corrected**: All critical errors fixed
- ✅ **Report Created**: Comprehensive verification report exists
- ✅ **Accuracy High**: Documentation accuracy > 95%

---

## Integration with Other Protocols

### Architecture Documentation Maintenance
- After updating architecture docs, verify accuracy with this protocol
- Ensures docs match actual implementation

### PR Documentation Update
- After documenting PRs, verify docs with this protocol
- Catches errors early

### Documentation Gap Analysis
- Use verification to find gaps (code exists but not documented)
- Use verification to find errors (docs wrong)

---

## Appendix: Automation Opportunities

### Automated Signature Extraction

**Idea**: Extract API signatures from source code automatically

**Tools**:
- Roslyn (C# code analysis)
- Reflection (runtime analysis)
- Custom parser

**Example**:
```csharp
// Tool to extract API signatures
var assembly = Assembly.LoadFrom("StreamTrackingService.dll");
var service = assembly.GetType("StreamTrackingService");
var methods = service.GetMethods(BindingFlags.Public | BindingFlags.Instance);

foreach (var method in methods)
{
    Console.WriteLine($"{method.ReturnType} {method.Name}(");
    foreach (var param in method.GetParameters())
    {
        Console.WriteLine($"  {param.ParameterType} {param.Name},");
    }
    Console.WriteLine(")");
}
```

### Automated Code Example Testing

**Idea**: Extract code examples from docs and compile them

**Process**:
1. Parse markdown for ```csharp blocks
2. Extract code examples
3. Create test project
4. Compile examples
5. Report errors

**Tool**: Custom script or tool

---

## Appendix: Quick Reference

```bash
# Phase 1: Scope
mkdir -p .verification
[Create verification-plan.md]

# Phase 2: Verify Entities
# Extract entity names from docs
grep "^### " docs/design/05-data-model.md | sed 's/### //; s/ Table//' > /tmp/doc-entities.txt
# Extract from code
find ./src/ -name "*Entity.cs" | sed 's|.*/||; s|\.cs||' > /tmp/code-entities.txt
# Compare
comm -23 /tmp/doc-entities.txt /tmp/code-entities.txt

# Phase 3: Verify APIs
[For each API, compare docs to source code]

# Phase 4: Verify Config
grep "Key\`:" docs/design/08-configuration-management.md | check against code

# Phase 5: Verify Examples
[Extract examples, try to compile]

# Phase 6: Create Report
[Aggregate findings in verification-report.md]

# Phase 7: Correct Errors
code docs/[file].md
[Fix errors]
git add docs/
git commit -m "docs: fix verification errors"
git push
```

---

**For AI Assistants**: This protocol should be executed when:
1. After creating/updating documentation
2. User requests "verify documentation"
3. Before important presentations/audits
4. Periodic quality reviews (quarterly)
5. After discovering documentation error

Use TodoWrite tool to track progress through all 7 phases.

**Typical Execution Time**: 2-8 hours for verification, 2-8 hours for corrections (varies by doc size and error count).
