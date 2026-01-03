# Architecture Documentation Maintenance Protocol

**Purpose**: Standardized protocol for maintaining architecture documentation in sync with code changes.

**Version**: 1.0
**Last Updated**: 2026-01-03
**Target**: Architecture documentation (C4 model and related docs)

---

## Overview

This protocol provides a systematic approach to:
1. **Keep architecture docs current** with code changes
2. **Update C4 model documents** when architecture evolves
3. **Maintain accuracy** between documentation and source code
4. **Track architectural decisions** over time
5. **Ensure consistency** across all architecture documents

**Key Concept**: Architecture documentation becomes stale quickly. This protocol ensures docs stay synchronized with the actual system implementation.

---

## When to Use This Protocol

Use this protocol when:
- ✅ Code changes affect system architecture
- ✅ New components/services are added
- ✅ APIs change or are added
- ✅ Database schema changes
- ✅ Security mechanisms change
- ✅ Deployment architecture changes
- ✅ Configuration management changes
- ✅ Periodic architecture review (quarterly/semi-annual)

Skip this protocol when:
- ❌ Changes are purely internal implementation (no architectural impact)
- ❌ Only test code changes
- ❌ Only documentation changes (no code changes)
- ❌ Trivial bug fixes with no behavior change

---

## Prerequisites

Before starting, verify:
- [ ] Architecture documentation exists in `docs/design/`
- [ ] C4 model structure is established
- [ ] You understand the code changes to document
- [ ] Access to source code and git history
- [ ] Familiarity with C4 model (System, Container, Component, Code levels)

---

## Phase 1: Identify Architecture Changes

### 1.1 Review Code Changes

**Purpose**: Understand what changed in the codebase

**Methods**:

**Option A: Review Recent Commits**
```bash
# Review commits since last architecture update
LAST_UPDATE="2025-12-01"  # From architecture doc metadata
git log --oneline --since="${LAST_UPDATE}" | tee .arch-review/commits.txt

# Find code files changed (not docs)
git log --oneline --name-only --since="${LAST_UPDATE}" -- '*.cs' '*.tsx' '*.sql' '*.csproj' | tee .arch-review/code-changes.txt
```

**Option B: Review Specific PRs/Commits**
```bash
# If using PR-DOCUMENTATION-UPDATE-PROTOCOL
# You already have a list of PRs to document
cat .pr-review/pr-inventory.md

# Extract architecture-impacting PRs
grep -E "Major Features|Security|Database|Infrastructure" .pr-review/pr-inventory.md > .arch-review/arch-prs.txt
```

**Option C: Review Since Last Architecture Doc Update**
```bash
# Check when architecture docs were last updated
git log -1 --format="%ai" docs/design/05-data-model.md

# Get all changes since then
LAST_COMMIT=$(git log -1 --format="%H" docs/design/05-data-model.md)
git log --oneline ${LAST_COMMIT}..HEAD
```

### 1.2 Categorize Changes by Architectural Impact

**Purpose**: Determine which architecture documents need updates

**Impact Categories**:

| Change Type | Architecture Impact | Affected C4 Level | Documents to Update |
|-------------|---------------------|-------------------|---------------------|
| **New Service/Application** | HIGH | Container (L2) | 02-containers.md, 03-components.md |
| **New API/Interface** | HIGH | Container/Component (L2/L3) | 04-apis.md, 03-components.md |
| **New Entity/Table** | MEDIUM | Component (L3) | 05-data-model.md |
| **Authentication Change** | CRITICAL | Component (L3) | 06-authentication-security.md |
| **New Integration** | HIGH | System Context (L1) | 01-system-context.md |
| **Schema Change** | MEDIUM | Component (L3) | 05-data-model.md |
| **Config Change** | MEDIUM | Component (L3) | 08-configuration-management.md |
| **Deployment Change** | HIGH | Container (L2) | 09-deployment-architecture.md |
| **Sync Mechanism Change** | MEDIUM | Component (L3) | 07-data-synchronization.md |
| **Test Strategy Change** | LOW | - | 10-testing-strategy.md |

### 1.3 Create Architecture Change Inventory

Create `.arch-review/architecture-changes.md`:

```markdown
# Architecture Changes Inventory

**Review Period**: [start-date] to [end-date]
**Commit Range**: [start-commit] to HEAD
**Total Changes Reviewed**: [count]
**Architecture Impact**: [HIGH/MEDIUM/LOW]

---

## Changes Requiring Architecture Documentation Updates

### HIGH Impact Changes

#### PR 12555 - Add OpenStreamActivity table
**Type**: New Entity/Table
**Architectural Impact**: MEDIUM
**Affected C4 Levels**: Component (L3)
**Documents to Update**:
- [ ] `docs/design/05-data-model.md` - Add OpenStreamActivity entity
- [ ] `docs/design/07-data-synchronization.md` - Add sync behavior

**Technical Details**:
- **New Table**: OpenStreamActivity with columns: Id, PatientId, UserId, OpenedAt, ClosedAt, StreamType
- **New Relationships**: BelongsTo Patient, BelongsTo User
- **Sync Impact**: New entity must sync between client/server

**Source Files**:
- `/current/src/Source/Database/Tables/OpenStreamActivity.cs:1`
- `/current/src/Source/Services/OpenStreamActivityService.cs:1`

#### PR 13040 - Password hashing enhancements
**Type**: Security Mechanism Change
**Architectural Impact**: CRITICAL
**Affected C4 Levels**: Component (L3)
**Documents to Update**:
- [ ] `docs/design/06-authentication-security.md` - Update password hashing section

**Technical Details**:
- **Algorithm Change**: SHA256 → PBKDF2 with salting
- **Impact**: More secure password storage
- **Breaking Change**: No (backward compatible)

**Source Files**:
- `/current/src/Source/Auth/PasswordHasher.cs:45`

[Continue for all architecture-impacting changes...]

---

## Summary by Architecture Document

| Document | Changes | Priority | Estimated Effort |
|----------|---------|----------|------------------|
| 01-system-context.md | 0 | - | 0h |
| 02-containers.md | 0 | - | 0h |
| 03-components.md | 1 | MEDIUM | 2h |
| 04-apis.md | 3 | HIGH | 4h |
| 05-data-model.md | 4 | HIGH | 5h |
| 06-authentication-security.md | 2 | CRITICAL | 3h |
| 07-data-synchronization.md | 2 | MEDIUM | 3h |
| 08-configuration-management.md | 2 | MEDIUM | 2h |
| 09-deployment-architecture.md | 1 | HIGH | 2h |
| 10-testing-strategy.md | 0 | - | 0h |
| **TOTAL** | **15** | - | **21h** |
```

---

## Phase 2: Update C4 Model Documents

### 2.1 Update System Context (Level 1)

**File**: `docs/design/01-system-context.md`

**When to Update**:
- New external systems integrated
- New user types added
- System boundaries change

**Update Process**:

1. **Review Current Document**:
   ```bash
   code docs/design/01-system-context.md
   ```

2. **Identify Changes**:
   - New external systems?
   - New user roles?
   - Changed system boundaries?

3. **Update Diagram** (if applicable):
   ```plantuml
   @startuml
   !include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

   Person(clinician, "Clinician", "Medical professional using the system")
   Person(admin, "Administrator", "System administrator")

   System(cadlink, "CadLink System", "Medical data management")

   System_Ext(pacs, "PACS", "Picture Archiving System")
   System_Ext(ehr, "EHR", "Electronic Health Record")

   Rel(clinician, cadlink, "Uses")
   Rel(admin, cadlink, "Administers")
   Rel(cadlink, pacs, "Exports images to")
   Rel(cadlink, ehr, "Integrates with")
   @enduml
   ```

4. **Update Text**:
   - Add new external systems to description
   - Update user roles if changed
   - Update relationships

5. **Add Metadata**:
   ```markdown
   **Last Updated**: [YYYY-MM-DD]
   **Updated For**: PR [numbers] - [brief description]
   **Verified Against**: [commit-hash]
   ```

### 2.2 Update Containers (Level 2)

**File**: `docs/design/02-containers.md`

**When to Update**:
- New applications/services added
- Container relationships change
- Technology stack changes

**Update Process**:

1. **Review Current Document**:
   ```bash
   code docs/design/02-containers.md
   ```

2. **Identify Container Changes**:
   - New containers (apps, services, databases)?
   - Changed container responsibilities?
   - Changed communication protocols?

3. **Update Diagram**:
   ```plantuml
   @startuml
   !include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

   Person(clinician, "Clinician")

   System_Boundary(c1, "CadLink System") {
       Container(client, "CadLink Client", "WinForms, .NET 4.8", "Desktop client application")
       Container(server, "CadLink Server", "WCF Service, .NET 4.8", "Server application")
       Container(manager, "CadManager", "WPF, .NET 4.8", "Management console")

       ContainerDb(clientDb, "Client Database", "SQLite", "Local patient data")
       ContainerDb(serverDb, "Server Database", "SQL Server", "Central patient data")
   }

   Rel(clinician, client, "Uses", "HTTPS")
   Rel(client, server, "Syncs data with", "WCF/HTTPS")
   Rel(client, clientDb, "Reads/Writes")
   Rel(server, serverDb, "Reads/Writes")
   Rel(manager, server, "Manages", "WCF/HTTPS")
   @enduml
   ```

4. **Update Container Descriptions**:
   ```markdown
   ### CadLink Client

   **Type**: Desktop Application
   **Technology**: WinForms, .NET Framework 4.8
   **Responsibilities**:
   - Patient data entry and viewing
   - Stream recording and playback
   - Local data storage
   - Data synchronization with server

   **Updated**: [YYYY-MM-DD] - Added stream tracking (PR 12555)
   **Source**: `/current/src/Source/CadLinkClient/Program.cs:1`
   ```

### 2.3 Update Components (Level 3)

**File**: `docs/design/03-components.md`

**When to Update**:
- New components/modules added
- Component responsibilities change
- Component relationships change

**Update Process**:

1. **Review Current Document**:
   ```bash
   code docs/design/03-components.md
   ```

2. **Add New Components**:
   ```markdown
   ### Stream Tracking Component

   **Purpose**: Track when users open and close patient streams
   **Location**: `/current/src/Source/Services/StreamTracking/`
   **Added**: PR 12555 (2025-11-15)

   **Responsibilities**:
   - Record stream open events
   - Record stream close events
   - Provide stream activity history
   - Support compliance auditing

   **Dependencies**:
   - Database (OpenStreamActivity table)
   - User Authentication (current user context)
   - Patient Service (patient validation)

   **Interfaces**:
   - `IStreamTrackingService` - Service interface
   - `IStreamActivityRepository` - Data access interface

   **Key Classes**:
   - `StreamTrackingService` - Main service implementation
   - `OpenStreamActivity` - Entity model
   - `StreamActivityRepository` - Data access layer

   **Source**: `/current/src/Source/Services/StreamTracking/StreamTrackingService.cs:1`
   ```

3. **Update Component Diagram** (if applicable):
   ```plantuml
   @startuml
   !include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Component.puml

   Container_Boundary(client, "CadLink Client") {
       Component(ui, "UI Layer", "WinForms", "User interface")
       Component(bizLogic, "Business Logic", "Services", "Business rules")
       Component(dataAccess, "Data Access", "Repositories", "Database access")
       Component(sync, "Sync Engine", "Sync Service", "Data synchronization")
       Component(streamTrack, "Stream Tracking", "Service", "Track stream activity")  # NEW

       ComponentDb(db, "Local Database", "SQLite")
   }

   Rel(ui, bizLogic, "Uses")
   Rel(bizLogic, dataAccess, "Uses")
   Rel(dataAccess, db, "Reads/Writes")
   Rel(sync, dataAccess, "Uses")
   Rel(streamTrack, dataAccess, "Uses")  # NEW
   Rel(bizLogic, streamTrack, "Uses")    # NEW
   @enduml
   ```

### 2.4 Update APIs (Level 3)

**File**: `docs/design/04-apis.md`

**When to Update**:
- New API endpoints added
- Existing APIs change signatures
- API contracts change

**Update Process**:

1. **Review Current Document**:
   ```bash
   code docs/design/04-apis.md
   ```

2. **Add New APIs**:
   ```markdown
   ### Stream Tracking API

   **Service**: `IStreamTrackingService`
   **Location**: `/current/src/Source/Interfaces/IStreamTrackingService.cs:1`
   **Added**: PR 12555 (2025-11-15)

   #### RecordStreamOpen

   **Signature**:
   ```csharp
   Task<Guid> RecordStreamOpen(Guid patientId, string streamType);
   ```

   **Description**: Records when a user opens a patient stream.

   **Parameters**:
   - `patientId` (Guid): ID of the patient whose stream is being opened
   - `streamType` (string): Type of stream (e.g., "EEG", "EMG", "PSG")

   **Returns**:
   - `Guid`: Activity ID for the opened stream

   **Exceptions**:
   - `ArgumentException`: If patientId is invalid
   - `UnauthorizedException`: If user not authorized for patient

   **Example**:
   ```csharp
   var activityId = await streamTrackingService.RecordStreamOpen(
       patientId: Guid.Parse("12345678-1234-1234-1234-123456789012"),
       streamType: "EEG"
   );
   ```

   **Source**: `/current/src/Source/Services/StreamTracking/StreamTrackingService.cs:45`

   [Continue for all methods...]
   ```

3. **Update API Index** (if exists):
   ```markdown
   ## API Index

   | Service | Method | Purpose | Added |
   |---------|--------|---------|-------|
   | StreamTracking | RecordStreamOpen | Record stream open | PR 12555 |
   | StreamTracking | RecordStreamClose | Record stream close | PR 12555 |
   | StreamTracking | GetStreamHistory | Get stream activity | PR 12555 |
   ```

### 2.5 Update Data Model

**File**: `docs/design/05-data-model.md`

**When to Update**:
- New database tables/entities added
- Table schemas change
- Relationships change
- Indexes added/changed

**Update Process**:

1. **Review Current Document**:
   ```bash
   code docs/design/05-data-model.md
   ```

2. **Add New Entities**:
   ```markdown
   ### OpenStreamActivity Table

   **Purpose**: Track when users open and close patient streams for auditing and compliance.
   **Added**: PR 12555 (2025-11-15)

   **Schema**:

   | Column | Type | Nullable | Default | Description |
   |--------|------|----------|---------|-------------|
   | Id | GUID | No | NewId() | Primary key |
   | PatientId | GUID | No | - | Foreign key to Patient.Id |
   | UserId | GUID | No | - | Foreign key to User.Id |
   | OpenedAt | DateTime | No | GetDate() | When stream was opened (UTC) |
   | ClosedAt | DateTime | Yes | NULL | When stream was closed (UTC), null if still open |
   | StreamType | VARCHAR(50) | No | - | Type of stream (EEG, EMG, PSG, etc.) |
   | ClientMachineName | VARCHAR(100) | Yes | NULL | Name of client machine |
   | ServerVersion | VARCHAR(20) | Yes | NULL | Server version at time of activity |

   **Constraints**:
   - Primary Key: Id
   - Foreign Key: PatientId → Patient.Id (ON DELETE CASCADE)
   - Foreign Key: UserId → User.Id (ON DELETE NO ACTION)
   - Check: ClosedAt IS NULL OR ClosedAt >= OpenedAt
   - Index: IX_OpenStreamActivity_PatientId (non-clustered)
   - Index: IX_OpenStreamActivity_UserId (non-clustered)
   - Index: IX_OpenStreamActivity_OpenedAt (non-clustered, DESC)

   **Relationships**:
   - BelongsTo: Patient (via PatientId)
   - BelongsTo: User (via UserId)

   **Synchronization**: See [07-data-synchronization.md#openstreamactivity](07-data-synchronization.md#openstreamactivity)

   **Entity Class**: `/current/src/Source/Database/Tables/OpenStreamActivity.cs:15`
   **Migration**: `/current/src/Source/Database/Migrations/20251115_AddOpenStreamActivity.sql:1`

   **HIPAA Compliance**:
   - Contains PHI: Yes (linked to Patient)
   - Audit Log: Yes (activity is itself an audit log)
   - Encryption: Follows database encryption policy
   ```

3. **Update ER Diagram** (if applicable):
   ```plantuml
   @startuml
   entity Patient {
     * Id : GUID
     --
     FirstName : VARCHAR(100)
     LastName : VARCHAR(100)
     DateOfBirth : Date
   }

   entity User {
     * Id : GUID
     --
     Username : VARCHAR(100)
     PasswordHash : VARCHAR(255)
   }

   entity OpenStreamActivity {
     * Id : GUID
     --
     * PatientId : GUID <<FK>>
     * UserId : GUID <<FK>>
     * OpenedAt : DateTime
     ClosedAt : DateTime
     * StreamType : VARCHAR(50)
     ClientMachineName : VARCHAR(100)
     ServerVersion : VARCHAR(20)
   }

   Patient ||--o{ OpenStreamActivity
   User ||--o{ OpenStreamActivity
   @enduml
   ```

### 2.6 Update Security Architecture

**File**: `docs/design/06-authentication-security.md`

**When to Update**:
- Authentication mechanisms change
- Authorization logic changes
- Encryption algorithms change
- Security policies change
- Compliance requirements change

**Update Process**:

1. **Review Current Document**:
   ```bash
   code docs/design/06-authentication-security.md
   ```

2. **Update Affected Sections**:
   ```markdown
   ### Password Hashing

   **Updated**: PR 13040 (2025-12-08) - Enhanced password hashing security

   **Algorithm**: PBKDF2 with HMAC-SHA256

   **Previous Algorithm** (deprecated as of 2025-12-08):
   - SHA256 simple hash (insecure, no salt)
   - Used until version 7.5.0
   - Automatically upgraded on user login

   **Current Implementation**:
   - **Algorithm**: PBKDF2 (Password-Based Key Derivation Function 2)
   - **Hash Function**: HMAC-SHA256
   - **Iterations**: 100,000 (as of 2025)
   - **Salt Size**: 128 bits (16 bytes), cryptographically random
   - **Hash Size**: 256 bits (32 bytes)

   **Storage Format**:
   ```
   [algorithm]$[iterations]$[base64(salt)]$[base64(hash)]
   Example: pbkdf2_sha256$100000$ABC123...$XYZ789...
   ```

   **Backward Compatibility**:
   - Old SHA256 hashes detected by format
   - Automatically upgraded on successful login
   - Old format: `sha256$[base64(hash)]`

   **Source**: `/current/src/Source/Auth/PasswordHasher.cs:45`

   **Security Rationale**:
   - PBKDF2 is NIST recommended (SP 800-132)
   - 100,000 iterations provides strong protection against brute force
   - Random salt prevents rainbow table attacks
   - HIPAA compliant password storage

   **Testing**:
   - Unit tests: `/current/src/Tests/Auth/PasswordHasherTests.cs:1`
   - Security scan: No vulnerabilities (Snyk, 2025-12-08)
   ```

### 2.7 Update Synchronization Architecture

**File**: `docs/design/07-data-synchronization.md`

**When to Update**:
- New entities added that sync
- Sync algorithms change
- Conflict resolution changes

**Update Process**:

1. **Review Current Document**:
   ```bash
   code docs/design/07-data-synchronization.md
   ```

2. **Add Sync Behavior for New Entities**:
   ```markdown
   ### OpenStreamActivity Synchronization

   **Added**: PR 12555 (2025-11-15)
   **Sync Direction**: Bidirectional (Client ↔ Server)
   **Sync Priority**: LOW (audit data)

   **Sync Behavior**:

   **Client → Server**:
   - When: Stream closed on client
   - What: OpenStreamActivity record with OpenedAt, ClosedAt, StreamType
   - Trigger: Automatic on stream close, manual on sync button
   - Batch: Up to 100 records per sync

   **Server → Client**:
   - When: Never (audit data created on client only)
   - Exception: If activity created on server for reporting, syncs to client

   **Conflict Resolution**:
   - Conflicts: Rare (activity created once, minimal updates)
   - Strategy: Server wins (server is authority for audit data)
   - If client has newer ClosedAt: Keep client version (client knows when closed)

   **SyncMetadata Tracking**:
   - Table: OpenStreamActivity
   - Tracked Fields: All
   - Last Modified: Updated on insert and ClosedAt update
   - Sync Status: Pending → InProgress → Complete

   **Performance**:
   - Average: 50 records per patient per day
   - Peak: 500 records per patient per day (heavy users)
   - Sync Time: <1 second for 100 records

   **Source**:
   - Client Sync: `/current/src/Source/Sync/EntitySyncHandlers/OpenStreamActivitySyncHandler.cs:1`
   - Server Sync: `/current/src/Source/Server/Sync/OpenStreamActivitySyncHandler.cs:1`
   ```

### 2.8 Update Configuration Management

**File**: `docs/design/08-configuration-management.md`

**When to Update**:
- New configuration settings added
- Configuration sources change
- Default values change

**Update Process**:

1. **Review Current Document**:
   ```bash
   code docs/design/08-configuration-management.md
   ```

2. **Add New Configuration Settings**:
   ```markdown
   ### Stream Tracking Configuration

   **Added**: PR 12555 (2025-11-15)
   **Section**: Client Configuration → Features

   #### EnableStreamTracking

   **Key**: `Features.EnableStreamTracking`
   **Type**: Boolean
   **Default**: `true`
   **Scope**: Client
   **Location**: `CadLinkClient.exe.config`

   **Description**: Enable or disable stream activity tracking.

   **Configuration**:
   ```xml
   <appSettings>
     <add key="Features.EnableStreamTracking" value="true" />
   </appSettings>
   ```

   **Behavior**:
   - `true`: Stream open/close events are recorded in OpenStreamActivity table
   - `false`: No stream tracking (for privacy or performance reasons)

   **Impact**:
   - Compliance: Disabling may affect HIPAA audit requirements
   - Performance: Minimal impact when enabled (<1ms per event)

   **Source**: `/current/src/Source/CadLinkClient/Configuration/FeatureFlags.cs:45`
   ```

### 2.9 Update Deployment Architecture

**File**: `docs/design/09-deployment-architecture.md`

**When to Update**:
- Deployment process changes
- Infrastructure requirements change
- New deployment targets added

### 2.10 Update Testing Strategy

**File**: `docs/design/10-testing-strategy.md`

**When to Update**:
- New test types added
- Test coverage strategy changes
- Testing tools change

---

## Phase 3: Cross-Reference and Consistency

### 3.1 Update Cross-References

**Purpose**: Ensure links between documents are current

**Process**:

1. **Find All Cross-References**:
   ```bash
   # Find all internal links
   grep -r "](docs/design/" docs/design/ | tee .arch-review/cross-refs.txt
   ```

2. **Verify Links**:
   ```bash
   # Check for broken links
   grep -r "](docs/design/" docs/design/ | sed 's/.*](\(docs\/design\/[^)]*\)).*/\1/' | while read link; do
       [ -f "$link" ] || echo "Broken link: $link"
   done
   ```

3. **Add New Cross-References**:
   - When adding new entity, link to sync docs
   - When adding new API, link to component docs
   - When adding new component, link to container docs

### 3.2 Ensure Terminology Consistency

**Purpose**: Use consistent terms across all docs

**Common Terminology Checklist**:

- [ ] Entity names match code (e.g., "OpenStreamActivity" not "StreamActivity")
- [ ] Service names match code (e.g., "StreamTrackingService")
- [ ] Table names match database (e.g., "Patient" not "Patients")
- [ ] Sync direction notation consistent (Client → Server, not Client->Server)
- [ ] Technology names correct (e.g., "SQLite" not "Sqlite")
- [ ] Consistent capitalization (e.g., "API" not "api" or "Api")

### 3.3 Update Metadata Consistently

**Purpose**: Maintain tracking information

**Required Metadata** (add to each updated document):

```markdown
---

**Document Version**: 2.3
**Last Updated**: 2026-01-03
**Updated For**: PR 12555, PR 13040, PR 13074 (20 PRs total)
**Reviewed By**: [Name]
**Next Review**: 2026-04-03 (quarterly)
**Verified Against**: Commit [hash]

---
```

---

## Phase 4: Validate Architecture Documentation

### 4.1 Architecture Documentation Checklist

**Purpose**: Ensure quality and completeness

**Validation Checklist**:

- [ ] **Accuracy**:
  - [ ] All entity names match code
  - [ ] All API signatures match code
  - [ ] All diagrams reflect current architecture
  - [ ] Source code references are correct (file:line)

- [ ] **Completeness**:
  - [ ] All new components documented
  - [ ] All new APIs documented
  - [ ] All new entities documented
  - [ ] All relationships shown

- [ ] **Consistency**:
  - [ ] Terminology consistent across all docs
  - [ ] Diagram notation consistent
  - [ ] Cross-references valid
  - [ ] Metadata complete

- [ ] **Quality**:
  - [ ] Code examples present where applicable
  - [ ] Diagrams render correctly
  - [ ] Markdown formatted correctly
  - [ ] No spelling/grammar errors

### 4.2 Source Code Verification

**Purpose**: Verify documentation matches actual code

**Process**: Use SOURCE-CODE-VERIFICATION-PROTOCOL

For each major change:
1. Locate source code file
2. Verify entity properties match docs
3. Verify API signatures match docs
4. Verify relationships match code
5. Add verification note

**Example Verification**:
```markdown
**Source Verification**: ✅ Verified against `/current/src/Source/Database/Tables/OpenStreamActivity.cs:15` on 2026-01-03
- Schema matches (7 columns)
- Relationships match (Patient, User)
- Constraints match (PKs, FKs, indexes)
```

### 4.3 Diagram Validation

**Purpose**: Ensure diagrams render and are accurate

**Process**:

1. **Test Diagram Rendering**:
   ```bash
   # If using PlantUML
   plantuml docs/design/diagrams/*.puml

   # Check for errors
   echo $?  # Should be 0
   ```

2. **Visual Review**:
   - Open rendered diagrams
   - Verify all boxes/connections present
   - Verify labels are correct
   - Verify legend (if applicable)

3. **Update Diagrams**:
   - Add new components/entities
   - Update relationships
   - Update labels

---

## Phase 5: Track Architectural Decisions

### 5.1 Document Architectural Decision Records (ADRs)

**Purpose**: Record why architectural decisions were made

**When to Create ADR**:
- Significant architectural change
- Technology choice
- Design pattern choice
- Security decision

**ADR Format**:

Create `docs/design/adr/0015-stream-activity-tracking.md`:

```markdown
# ADR 0015: Stream Activity Tracking

**Status**: Accepted
**Date**: 2025-11-15
**Deciders**: [Names]
**Updated**: 2025-11-15

## Context

We need to track when users open and close patient streams for HIPAA compliance and auditing purposes.

## Decision

We will implement a dedicated OpenStreamActivity table to track stream open/close events.

## Rationale

### Options Considered

1. **Dedicated Table** (CHOSEN)
   - ✅ Clean separation of concerns
   - ✅ Easy to query audit trail
   - ✅ Supports compliance requirements
   - ✅ Minimal performance impact
   - ❌ Additional table to maintain

2. **Generic Audit Log**
   - ✅ Centralized audit logging
   - ❌ Difficult to query specific stream events
   - ❌ Performance issues with large audit table
   - ❌ Mixing stream events with other audit events

3. **Patient Table Extension**
   - ❌ Pollutes Patient table
   - ❌ Can't track multiple concurrent streams
   - ❌ Can't track historical data

### Why Dedicated Table

- HIPAA requires audit trail of PHI access
- Compliance officers need easy reporting
- Performance isolation from Patient table
- Future-proof for additional tracking fields

## Consequences

### Positive

- ✅ Clear audit trail for compliance
- ✅ Easy to query and report
- ✅ Minimal performance impact
- ✅ Extensible for future needs

### Negative

- ⚠️ Additional table to maintain
- ⚠️ Additional sync complexity
- ⚠️ Small storage overhead (~50 records/patient/day)

### Mitigation

- Automated sync handles complexity
- Archival strategy for old records (>1 year)
- Indexes optimize query performance

## Implementation

**PR**: #12555
**Code**: `/current/src/Source/Database/Tables/OpenStreamActivity.cs:15`
**Documentation**: `docs/design/05-data-model.md#openstreamactivity`

## Related Decisions

- ADR 0003: Audit Logging Strategy
- ADR 0008: HIPAA Compliance Architecture

---

**Created**: 2025-11-15
**Last Reviewed**: 2025-11-15
**Next Review**: 2026-05-15 (6 months)
```

### 5.2 Update ADR Index

**File**: `docs/design/adr/README.md`

```markdown
# Architectural Decision Records

## Active ADRs

| ADR | Title | Status | Date | Topic |
|-----|-------|--------|------|-------|
| 0015 | Stream Activity Tracking | Accepted | 2025-11-15 | Data Model |
| 0014 | Password Hashing Enhancement | Accepted | 2025-12-08 | Security |
| [Previous ADRs...] | | | | |

## ADR Index by Topic

### Data Model
- ADR 0015: Stream Activity Tracking
- ADR 0007: Entity Relationship Design

### Security
- ADR 0014: Password Hashing Enhancement
- ADR 0003: Audit Logging Strategy

[Continue...]
```

---

## Phase 6: Commit Architecture Updates

### 6.1 Commit Strategy

**Purpose**: Commit architecture updates with clear messages

```bash
# Stage architecture docs
git add docs/design/

# Commit architecture updates
git commit -m "docs: update architecture for PRs [list]

Updated architecture documentation for [X] code changes:

Data Model (05-data-model.md):
- Added OpenStreamActivity entity (PR 12555)
- Updated ForeignFile entity with subfolders (PR 12997)

Security (06-authentication-security.md):
- Updated password hashing to PBKDF2 (PR 13040)
- Added ProtectedData utilities (PR 13032)

APIs (04-apis.md):
- Added StreamTracking API (PR 12555)
- Added Configuration API (PR 13083)

Synchronization (07-data-synchronization.md):
- Added OpenStreamActivity sync (PR 12555)
- Updated SyncMetadata thread safety (PR 13074)

Configuration (08-configuration-management.md):
- Added Raygun API key config (PR 13083)
- Added SystemMetrics retention setting (PR 12954)

Total changes: [X] PRs, [Y] documents updated
Verified against: commit [hash]

Source verification completed: 2026-01-03"

# If ADRs created
git add docs/design/adr/
git commit -m "docs: add ADRs for architectural decisions

Created ADRs:
- ADR 0015: Stream Activity Tracking (PR 12555)
- ADR 0014: Password Hashing Enhancement (PR 13040)

Updated ADR index with new decisions"

# Push
git push origin $(git branch --show-current)
```

---

## Success Metrics

This protocol is successful when:

- ✅ **All Architecture Docs Updated**: Changes reflected in all affected docs
- ✅ **Source Code Verified**: Documentation matches actual code
- ✅ **Cross-References Valid**: All internal links work
- ✅ **Diagrams Current**: All diagrams reflect current architecture
- ✅ **Metadata Complete**: Last updated, verified against, etc.
- ✅ **ADRs Recorded**: Significant decisions documented
- ✅ **Consistency Maintained**: Terminology and format consistent

---

## Integration with Other Protocols

### PR Documentation Integration
- Use PR-DOCUMENTATION-UPDATE-PROTOCOL to identify changes
- Use this protocol to update architecture docs

### Source Code Verification Integration
- Use SOURCE-CODE-VERIFICATION-PROTOCOL to verify accuracy
- Ensure documentation matches actual implementation

---

## Appendix: Quick Reference

```bash
# Phase 1: Identify Changes
mkdir -p .arch-review
git log --oneline --since="2025-12-01" > .arch-review/commits.txt
[Create architecture-changes.md inventory]

# Phase 2: Update C4 Docs
code docs/design/01-system-context.md
code docs/design/02-containers.md
code docs/design/03-components.md
code docs/design/04-apis.md
code docs/design/05-data-model.md
code docs/design/06-authentication-security.md
code docs/design/07-data-synchronization.md
code docs/design/08-configuration-management.md
code docs/design/09-deployment-architecture.md
code docs/design/10-testing-strategy.md

# Phase 3: Cross-Reference
grep -r "](docs/design/" docs/design/ | check for broken links

# Phase 4: Validate
[Run SOURCE-CODE-VERIFICATION-PROTOCOL]
[Check diagrams render]

# Phase 5: Track Decisions
[Create ADRs for significant changes]

# Phase 6: Commit
git add docs/design/
git commit -m "docs: update architecture for PRs [list]"
git push
```

---

**For AI Assistants**: This protocol should be executed when:
1. Code changes affect architecture
2. User requests "update architecture docs"
3. After using PR-DOCUMENTATION-UPDATE-PROTOCOL
4. Periodic architecture review

Use TodoWrite tool to track progress through all 6 phases.

**Typical Execution Time**: 4-20 hours depending on number of changes.
