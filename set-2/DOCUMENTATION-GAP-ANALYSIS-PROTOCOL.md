# Documentation Gap Analysis Protocol

**Purpose**: Standardized protocol for identifying, measuring, and closing documentation gaps in technical projects.

**Version**: 1.0
**Last Updated**: 2026-01-03
**Target**: All project documentation

---

## Overview

This protocol provides a systematic approach to:
1. **Identify documentation gaps** - What should be documented but isn't
2. **Measure gap severity** - Quantify the documentation coverage
3. **Prioritize gap closure** - Determine what to fix first
4. **Create remediation plan** - Plan and track gap closure
5. **Validate completeness** - Ensure gaps are properly closed

**Key Concept**: Documentation gaps occur when code/features exist but documentation is missing, incomplete, or incorrect. This protocol helps systematically find and fix those gaps.

---

## When to Use This Protocol

Use this protocol when:
- ✅ Starting documentation for an existing codebase
- ✅ Code has changed significantly without documentation updates
- ✅ New team members struggle to understand the system
- ✅ Preparing for audit or compliance review
- ✅ Conducting periodic documentation quality review
- ✅ After major refactoring or feature additions
- ✅ Before major release

Skip this protocol when:
- ❌ Documentation is already current and verified
- ❌ Codebase is very small (< 1000 LOC)
- ❌ Documentation just created and verified

---

## Prerequisites

Before starting, verify:
- [ ] Access to complete source code
- [ ] Access to git history
- [ ] Understanding of system architecture
- [ ] List of expected documentation (from protocol/standards)
- [ ] Time allocated for analysis (typically 4-16 hours)
- [ ] Stakeholder buy-in for remediation work

---

## Phase 1: Define Documentation Scope

### 1.1 Identify Documentation Types

**Purpose**: Define what documentation should exist

**Standard Documentation Types**:

| Type | Purpose | Examples | Mandatory? |
|------|---------|----------|------------|
| **Architecture** | System design, structure | C4 model, diagrams | ✅ Yes |
| **API Documentation** | Public interfaces, contracts | Method signatures, parameters | ✅ Yes |
| **User Guides** | End-user instructions | How-to guides, tutorials | ✅ Yes |
| **Developer Guides** | Development setup, workflows | Build instructions, coding standards | ✅ Yes |
| **Data Model** | Database schema, entities | Table definitions, relationships | ✅ Yes |
| **Configuration** | Setup, settings | Config file documentation, options | ✅ Yes |
| **Deployment** | Installation, deployment | Deployment steps, requirements | ✅ Yes |
| **Security** | Security architecture, practices | Authentication, authorization, encryption | ✅ Yes |
| **Testing** | Test strategy, procedures | Test types, coverage, execution | ⚠️ Recommended |
| **Troubleshooting** | Common issues, solutions | Error messages, debugging | ⚠️ Recommended |
| **Release Notes** | Version history, changes | What's new, breaking changes | ⚠️ Recommended |
| **Code Comments** | Inline code documentation | Complex logic explanation | ⚠️ Recommended |

### 1.2 Define Documentation Standards

**Purpose**: Establish what "complete" documentation means

Create `.gap-analysis/documentation-standards.md`:

```markdown
# Documentation Standards

## Architecture Documentation

**Required Components**:
- [ ] System Context diagram
- [ ] Container diagram
- [ ] Component diagrams for each major subsystem
- [ ] API documentation for public interfaces
- [ ] Data model documentation
- [ ] Security architecture
- [ ] Configuration management
- [ ] Deployment architecture

**Quality Standards**:
- Source code references (file:line)
- Code examples where applicable
- Diagrams (PlantUML, Mermaid, or images)
- Updated within 30 days of code changes
- Peer reviewed

## API Documentation

**Required Components**:
- [ ] Method/function signature
- [ ] Parameter descriptions with types
- [ ] Return value description
- [ ] Exception documentation
- [ ] Usage examples
- [ ] Since version (when added)

**Quality Standards**:
- All public APIs documented
- Examples tested and verified
- Updated with each API change

## User Guides

**Required Components**:
- [ ] Feature overview
- [ ] Step-by-step instructions
- [ ] Screenshots/diagrams
- [ ] Common workflows
- [ ] Troubleshooting section

**Quality Standards**:
- User-focused language (not technical)
- Screenshots current (< 6 months old)
- Validated by actual users

[Continue for all documentation types...]
```

### 1.3 Create Documentation Inventory

**Purpose**: List all current documentation

```bash
# Create working directory
mkdir -p .gap-analysis

# List all markdown documentation
find docs/ -name "*.md" | sort > .gap-analysis/current-docs.txt

# Count documentation files by type
echo "Architecture Docs:"
find docs/design/ -name "*.md" | wc -l

echo "User Guides:"
find docs/user-guide/ -name "*.md" | wc -l

echo "API Docs:"
find docs/api/ -name "*.md" | wc -l

# List documentation file sizes
find docs/ -name "*.md" -exec wc -l {} \; | sort -rn > .gap-analysis/doc-sizes.txt
```

---

## Phase 2: Identify Documentation Gaps

### 2.1 Architecture Documentation Gaps

**Purpose**: Find missing architecture documentation

**Process**:

1. **Review Expected Architecture Docs**:
   - System Context (C4 L1)
   - Container Diagram (C4 L2)
   - Component Diagrams (C4 L3)
   - API Documentation
   - Data Model
   - Security Architecture
   - Configuration Management
   - Deployment Architecture

2. **Check Existence**:
   ```bash
   # Check for architecture docs
   ls docs/design/01-system-context.md  # Should exist
   ls docs/design/02-containers.md      # Should exist
   ls docs/design/03-components.md      # Should exist
   ls docs/design/04-apis.md            # Should exist
   ls docs/design/05-data-model.md      # Should exist
   ls docs/design/06-authentication-security.md  # Should exist
   ls docs/design/07-data-synchronization.md     # Should exist (if applicable)
   ls docs/design/08-configuration-management.md # Should exist
   ls docs/design/09-deployment-architecture.md  # Should exist
   ls docs/design/10-testing-strategy.md         # Should exist
   ```

3. **Record Gaps**:

   Create `.gap-analysis/architecture-gaps.md`:

   ```markdown
   # Architecture Documentation Gaps

   ## Missing Documents
   - [ ] `docs/design/XX-[missing-doc].md` - [Purpose]

   ## Incomplete Documents

   ### docs/design/05-data-model.md
   **Current State**: Exists, 250 lines
   **Gaps Identified**:
   - ❌ Missing entities: [List entities found in code but not documented]
   - ❌ Missing relationships: [List relationships not shown]
   - ⚠️ Outdated: Last updated 6 months ago, code has changed
   - ❌ No migration history

   **Gap Severity**: HIGH
   **Estimated Effort**: 4 hours

   ### docs/design/06-authentication-security.md
   **Current State**: Exists, 180 lines
   **Gaps Identified**:
   - ❌ Missing password policy documentation
   - ❌ Missing session management details
   - ⚠️ Encryption algorithm outdated (references old algorithm)

   **Gap Severity**: CRITICAL (Security)
   **Estimated Effort**: 3 hours
   ```

### 2.2 API Documentation Gaps

**Purpose**: Find missing API documentation

**Process**:

1. **Identify Public APIs**:
   ```bash
   # Find public classes
   grep -r "public class" --include="*.cs" ./src/ | wc -l

   # Find public methods
   grep -r "public.*(" --include="*.cs" ./src/ | grep -v "get;" | grep -v "set;" | wc -l
   ```

2. **Check API Documentation**:
   ```bash
   # Check for API docs directory
   ls docs/api/

   # List API doc files
   find docs/api/ -name "*.md" | sort
   ```

3. **Identify Undocumented APIs**:
   ```bash
   # Compare code files to doc files
   # List all service/API classes
   find ./src/ -name "*Service.cs" -o -name "*Controller.cs" -o -name "*Api.cs" | sed 's|.*/||; s|\.cs||' | sort > /tmp/code-apis.txt

   # List documented APIs
   find docs/api/ -name "*.md" | sed 's|.*/||; s|\.md||' | sort > /tmp/doc-apis.txt

   # Find undocumented APIs
   comm -23 /tmp/code-apis.txt /tmp/doc-apis.txt > .gap-analysis/undocumented-apis.txt

   # Show count
   wc -l .gap-analysis/undocumented-apis.txt
   ```

4. **Record Gaps**:

   Create `.gap-analysis/api-gaps.md`:

   ```markdown
   # API Documentation Gaps

   ## Summary
   **Total Public APIs**: [count from code]
   **Documented APIs**: [count from docs]
   **Undocumented APIs**: [count]
   **Documentation Coverage**: [percentage]%

   ## Undocumented APIs (Critical)

   ### PatientService
   **File**: `/src/Services/PatientService.cs:1`
   **Public Methods**: 12
   **Documented Methods**: 0
   **Gap Severity**: CRITICAL
   **Reason**: Core business logic, widely used

   **Missing Documentation**:
   - [ ] `GetPatient(Guid id)` - Retrieve patient by ID
   - [ ] `CreatePatient(Patient patient)` - Create new patient
   - [ ] `UpdatePatient(Patient patient)` - Update existing patient
   - [ ] `DeletePatient(Guid id)` - Delete patient
   - [ ] [List all undocumented methods]

   **Estimated Effort**: 6 hours

   ### StreamService
   **File**: `/src/Services/StreamService.cs:1`
   **Public Methods**: 8
   **Documented Methods**: 0
   **Gap Severity**: HIGH
   **Reason**: Important feature

   [Continue for all undocumented APIs...]

   ## Partially Documented APIs

   ### UserService
   **File**: `/src/Services/UserService.cs:1`
   **Public Methods**: 10
   **Documented Methods**: 6
   **Documentation Coverage**: 60%
   **Gap Severity**: MEDIUM

   **Missing Documentation**:
   - [ ] `ResetPassword(Guid userId)` - Reset user password
   - [ ] `LockAccount(Guid userId)` - Lock user account
   - [ ] `UnlockAccount(Guid userId)` - Unlock user account
   - [ ] `GetUserPreferences(Guid userId)` - Get user preferences

   **Estimated Effort**: 2 hours
   ```

### 2.3 Data Model Documentation Gaps

**Purpose**: Find missing database/entity documentation

**Process**:

1. **Identify Database Tables**:
   ```bash
   # Find SQL files
   find ./src/ -name "*.sql" | grep -i "create table" | wc -l

   # Find entity classes
   grep -r "public class.*:" --include="*.cs" ./src/ | grep -E "Entity|Model|Table" | wc -l
   ```

2. **Check Data Model Documentation**:
   ```bash
   # Check data model doc
   ls docs/design/05-data-model.md
   ```

3. **Compare Entities to Documentation**:
   ```bash
   # List entity classes
   find ./src/ -path "*/Database/Tables/*.cs" -o -path "*/Entities/*.cs" | sed 's|.*/||; s|\.cs||' | sort > /tmp/code-entities.txt

   # Extract documented entities from data model doc
   grep "^### " docs/design/05-data-model.md | sed 's/### //; s/ Table//; s/ Entity//' | sort > /tmp/doc-entities.txt

   # Find undocumented entities
   comm -23 /tmp/code-entities.txt /tmp/doc-entities.txt > .gap-analysis/undocumented-entities.txt

   # Show count
   wc -l .gap-analysis/undocumented-entities.txt
   ```

4. **Record Gaps**:

   Create `.gap-analysis/data-model-gaps.md`:

   ```markdown
   # Data Model Documentation Gaps

   ## Summary
   **Total Entities**: [count from code]
   **Documented Entities**: [count from docs]
   **Undocumented Entities**: [count]
   **Documentation Coverage**: [percentage]%

   ## Undocumented Entities

   ### Patient (CRITICAL)
   **File**: `/src/Database/Tables/Patient.cs:1`
   **Columns**: 25
   **Relationships**: HasMany Studies, HasMany ForeignFiles
   **Gap Severity**: CRITICAL
   **Reason**: Core business entity

   **Missing Documentation**:
   - Table schema (columns, types, constraints)
   - Relationships with other tables
   - Synchronization behavior
   - HIPAA compliance notes

   **Estimated Effort**: 3 hours

   ### OpenStreamActivity (HIGH)
   **File**: `/src/Database/Tables/OpenStreamActivity.cs:1`
   **Columns**: 7
   **Relationships**: BelongsTo Patient, BelongsTo User
   **Gap Severity**: HIGH
   **Reason**: New feature (PR 12555), recently added

   **Missing Documentation**:
   - Table schema
   - Purpose and usage
   - Synchronization behavior

   **Estimated Effort**: 2 hours

   [Continue for all undocumented entities...]
   ```

### 2.4 User Guide Documentation Gaps

**Purpose**: Find missing user-facing documentation

**Process**:

1. **Identify User-Facing Features**:
   - Review UI components
   - Review application screens
   - Review business workflows
   - Review configuration options

2. **Check User Guide Coverage**:
   ```bash
   # List user guide docs
   find docs/user-guide/ -name "*.md" | sort

   # Check for specific feature guides
   ls docs/user-guide/patient-management.md
   ls docs/user-guide/stream-recording.md
   ls docs/user-guide/data-export.md
   # etc.
   ```

3. **Record Gaps**:

   Create `.gap-analysis/user-guide-gaps.md`:

   ```markdown
   # User Guide Documentation Gaps

   ## Summary
   **Major Features**: [count]
   **Documented Features**: [count]
   **Undocumented Features**: [count]
   **Documentation Coverage**: [percentage]%

   ## Undocumented Features

   ### Stream Recording Workflow (HIGH)
   **Feature**: Record patient streams (EEG, EMG, etc.)
   **Complexity**: HIGH
   **Users**: All clinical users
   **Gap Severity**: HIGH
   **Reason**: Core workflow, no user guide exists

   **Missing Documentation**:
   - [ ] How to start a stream recording
   - [ ] How to configure stream settings
   - [ ] How to stop/save a recording
   - [ ] How to review recorded streams
   - [ ] Troubleshooting common issues

   **Estimated Effort**: 8 hours (includes screenshots)

   ### Data Export (MEDIUM)
   **Feature**: Export patient data to external formats
   **Complexity**: MEDIUM
   **Users**: Administrators, researchers
   **Gap Severity**: MEDIUM
   **Reason**: Complex feature, only basic docs exist

   **Missing Documentation**:
   - [ ] Export format options
   - [ ] Data filtering/selection
   - [ ] Scheduling automated exports
   - [ ] Export troubleshooting

   **Estimated Effort**: 4 hours

   [Continue for all undocumented features...]
   ```

### 2.5 Additional Gap Categories

**Other Documentation Gaps to Check**:

- **Configuration Documentation**: Missing config file documentation
- **Deployment Documentation**: Missing deployment steps
- **Security Documentation**: Missing security practices
- **Troubleshooting Documentation**: Missing error messages/solutions
- **Release Notes**: Missing version history
- **Migration Guides**: Missing upgrade instructions
- **Code Comments**: Missing complex logic explanations

---

## Phase 3: Measure Documentation Coverage

### 3.1 Calculate Coverage Metrics

**Purpose**: Quantify documentation gaps

Create `.gap-analysis/coverage-metrics.md`:

```markdown
# Documentation Coverage Metrics

**Analysis Date**: [YYYY-MM-DD]
**Codebase Version**: [commit-hash]

---

## Overall Coverage

| Category | Total Items | Documented | Undocumented | Coverage % | Gap Severity |
|----------|-------------|------------|--------------|------------|--------------|
| **Architecture** | 10 docs | 8 | 2 | 80% | MEDIUM |
| **APIs** | 45 APIs | 18 | 27 | 40% | CRITICAL |
| **Data Model** | 32 entities | 20 | 12 | 63% | HIGH |
| **User Guides** | 15 features | 5 | 10 | 33% | HIGH |
| **Configuration** | 50 settings | 25 | 25 | 50% | MEDIUM |
| **Security** | 8 topics | 6 | 2 | 75% | HIGH |
| **Deployment** | 5 docs | 4 | 1 | 80% | LOW |
| **Testing** | 3 docs | 2 | 1 | 67% | LOW |
| **TOTAL** | 168 items | 88 | 80 | **52%** | **HIGH** |

---

## Coverage by Priority

### CRITICAL Gaps (Fix Immediately)
**Count**: 15 items
**Estimated Effort**: 30 hours

| Item | Type | Reason | Effort |
|------|------|--------|--------|
| PatientService API | API | Core business logic | 6h |
| Patient Entity | Data Model | Core entity | 3h |
| Authentication Security | Security | Security critical | 4h |
| [List all critical gaps] | | | |

### HIGH Gaps (Fix This Sprint)
**Count**: 35 items
**Estimated Effort**: 65 hours

| Item | Type | Reason | Effort |
|------|------|--------|--------|
| StreamService API | API | Important feature | 4h |
| Stream Recording Guide | User Guide | Core workflow | 8h |
| [List all high gaps] | | | |

### MEDIUM Gaps (Fix This Month)
**Count**: 20 items
**Estimated Effort**: 35 hours

### LOW Gaps (Backlog)
**Count**: 10 items
**Estimated Effort**: 15 hours

---

## Trend Analysis

**Previous Analysis**: [date] - Coverage: XX%
**Current Analysis**: [date] - Coverage: 52%
**Change**: [+/-]X%

**Trend**: [Improving/Declining/Stable]

---

## Gap Distribution by Category

```
Architecture:    ████████░░ 80%
APIs:            ████░░░░░░ 40% ⚠️
Data Model:      ██████░░░░ 63%
User Guides:     ███░░░░░░░ 33% ⚠️
Configuration:   █████░░░░░ 50%
Security:        ███████░░░ 75%
Deployment:      ████████░░ 80%
Testing:         ██████░░░░ 67%
```

---

## Impact Assessment

### Business Impact
**HIGH**: User guides and API documentation gaps affect user productivity and developer onboarding

### Compliance Impact
**MEDIUM**: Security documentation gaps may affect compliance audits

### Maintenance Impact
**HIGH**: Architecture gaps make system changes risky and time-consuming

---

## Recommendations

1. **Immediate Actions** (CRITICAL gaps):
   - Document PatientService API (6h)
   - Document Patient entity (3h)
   - Update Authentication Security docs (4h)
   - **Total**: 13 hours

2. **Short-Term Actions** (HIGH gaps):
   - Document major APIs (30h)
   - Create user guides for core workflows (25h)
   - Update data model docs (10h)
   - **Total**: 65 hours

3. **Long-Term Actions** (MEDIUM/LOW gaps):
   - Complete remaining API docs (20h)
   - Create advanced user guides (15h)
   - **Total**: 35 hours

**Grand Total**: 113 hours (~14 days)
```

### 3.2 Visualize Coverage Gaps

**Purpose**: Create visual representation of gaps

```bash
# Create simple bar chart in markdown
cat > .gap-analysis/coverage-chart.md << 'EOF'
# Documentation Coverage Chart

```
Category               Coverage
─────────────────────  ─────────────────────
Architecture           ████████░░ 80%
APIs                   ████░░░░░░ 40%  ⚠️ CRITICAL
Data Model             ██████░░░░ 63%
User Guides            ███░░░░░░░ 33%  ⚠️ CRITICAL
Configuration          █████░░░░░ 50%
Security               ███████░░░ 75%
Deployment             ████████░░ 80%
Testing                ██████░░░░ 67%
─────────────────────  ─────────────────────
OVERALL                █████░░░░░ 52%
```

**Legend**:
- ████████░░ 70-100% - Good coverage
- █████░░░░░ 50-69% - Moderate coverage
- ███░░░░░░░ <50% - Poor coverage ⚠️
EOF
```

---

## Phase 4: Prioritize Gap Closure

### 4.1 Apply Prioritization Framework

**Purpose**: Determine what to fix first

**Prioritization Criteria**:

| Priority | Criteria | Timeline | Examples |
|----------|----------|----------|----------|
| **CRITICAL** | Security, Compliance, Core Business Logic | Fix immediately (24-48h) | Authentication docs, HIPAA compliance, Core APIs |
| **HIGH** | Major features, Public APIs, User-facing docs | Fix this sprint (1-2 weeks) | Feature guides, Major API docs, Data model |
| **MEDIUM** | Internal APIs, Configuration, Minor features | Fix this month (2-4 weeks) | Helper APIs, Config options, Admin features |
| **LOW** | Internal implementation, Advanced features | Backlog (1-3 months) | Implementation details, Advanced workflows |

**Decision Matrix**:

```
Impact/Audience Matrix:

               │ External  │ Internal  │ Advanced
               │ Users     │ Developers│ Users
───────────────┼───────────┼───────────┼──────────
Security       │ CRITICAL  │ CRITICAL  │ HIGH
Core Features  │ CRITICAL  │ HIGH      │ MEDIUM
Major Features │ HIGH      │ HIGH      │ MEDIUM
Minor Features │ MEDIUM    │ MEDIUM    │ LOW
Implementation │ LOW       │ MEDIUM    │ LOW
```

### 4.2 Create Prioritized Gap List

Create `.gap-analysis/prioritized-gaps.md`:

```markdown
# Prioritized Documentation Gaps

---

## CRITICAL Priority (Fix in 24-48 hours)

**Total Items**: 15
**Total Effort**: 30 hours
**Deadline**: [date + 2 days]

### 1. Authentication Security Documentation (CRITICAL - Security)
**File**: `docs/design/06-authentication-security.md`
**Gap**: Missing password policy, session management, encryption algorithms
**Impact**: Security audit failure risk, compliance issues
**Audience**: Developers, Security team, Auditors
**Effort**: 4 hours
**Owner**: [Assign]
**Due**: [Date]

### 2. PatientService API Documentation (CRITICAL - Core Business Logic)
**File**: `docs/api/patient-service.md` (NEW)
**Gap**: No documentation for 12 public methods
**Impact**: Developer onboarding, integration issues
**Audience**: Developers (internal/external)
**Effort**: 6 hours
**Owner**: [Assign]
**Due**: [Date]

[Continue for all CRITICAL gaps...]

---

## HIGH Priority (Fix in 1-2 weeks)

**Total Items**: 35
**Total Effort**: 65 hours
**Deadline**: [date + 2 weeks]

### 1. Stream Recording User Guide (HIGH - Core Workflow)
**File**: `docs/user-guide/stream-recording.md` (NEW)
**Gap**: No user guide for core clinical workflow
**Impact**: User confusion, support burden, training costs
**Audience**: Clinical users (external)
**Effort**: 8 hours (includes screenshots)
**Owner**: [Assign]
**Due**: [Date]

[Continue for all HIGH gaps...]

---

## MEDIUM Priority (Fix in 2-4 weeks)

**Total Items**: 20
**Total Effort**: 35 hours
**Deadline**: [date + 1 month]

[List MEDIUM gaps...]

---

## LOW Priority (Backlog)

**Total Items**: 10
**Total Effort**: 15 hours
**Deadline**: Backlog

[List LOW gaps...]
```

---

## Phase 5: Create Remediation Plan

### 5.1 Break Down into Sprints

**Purpose**: Create actionable plan with milestones

Create `.gap-analysis/remediation-plan.md`:

```markdown
# Documentation Gap Remediation Plan

**Plan Created**: [YYYY-MM-DD]
**Total Gaps**: 80 items
**Total Effort**: 145 hours (~18 days)
**Target Completion**: [date + 6 weeks]

---

## Sprint 1: Critical Gaps (Week 1)

**Focus**: Security, Core APIs, Core Entities
**Effort**: 30 hours
**Owner**: [Assign team/person]

### Tasks

- [ ] **Day 1-2**: Authentication Security Documentation (4h)
  - Update password policy documentation
  - Document session management
  - Update encryption algorithms
  - File: `docs/design/06-authentication-security.md`

- [ ] **Day 2-4**: PatientService API Documentation (6h)
  - Document all 12 public methods
  - Add usage examples
  - Add error handling docs
  - File: `docs/api/patient-service.md` (NEW)

- [ ] **Day 4-5**: Patient Entity Documentation (3h)
  - Document table schema
  - Document relationships
  - Add HIPAA compliance notes
  - File: `docs/design/05-data-model.md`

[Continue for all Sprint 1 tasks...]

**Success Criteria**:
- ✅ All CRITICAL gaps closed
- ✅ Security audit risk mitigated
- ✅ Core APIs documented

---

## Sprint 2: High Priority APIs (Week 2-3)

**Focus**: Major APIs, Services
**Effort**: 35 hours
**Owner**: [Assign]

### Tasks

- [ ] **Week 2**: StreamService API (4h)
- [ ] **Week 2**: UserService API (3h)
- [ ] **Week 2**: StudyService API (5h)
- [ ] **Week 3**: ConfigurationService API (3h)
- [ ] **Week 3**: SyncService API (4h)

[Continue...]

**Success Criteria**:
- ✅ All major APIs documented
- ✅ API coverage > 70%

---

## Sprint 3: User Guides (Week 3-4)

**Focus**: User-facing documentation
**Effort**: 40 hours
**Owner**: [Assign]

### Tasks

- [ ] **Week 3-4**: Stream Recording User Guide (8h)
  - Step-by-step instructions
  - Screenshots
  - Troubleshooting
  - File: `docs/user-guide/stream-recording.md` (NEW)

- [ ] **Week 4**: Patient Management User Guide (6h)
- [ ] **Week 4**: Data Export User Guide (4h)

[Continue...]

**Success Criteria**:
- ✅ All core workflows documented
- ✅ User guide coverage > 60%
- ✅ Screenshots current

---

## Sprint 4: Data Model & Configuration (Week 5)

**Focus**: Data model completeness, configuration docs
**Effort**: 25 hours
**Owner**: [Assign]

[Continue...]

---

## Sprint 5: Remaining Gaps (Week 6)

**Focus**: MEDIUM priority gaps, polish
**Effort**: 15 hours
**Owner**: [Assign]

[Continue...]

---

## Milestones

| Milestone | Date | Deliverable | Status |
|-----------|------|-------------|--------|
| Sprint 1 Complete | [Week 1 end] | All CRITICAL gaps closed | 🔴 Pending |
| Sprint 2 Complete | [Week 3 end] | All HIGH API gaps closed | 🔴 Pending |
| Sprint 3 Complete | [Week 4 end] | All HIGH user guide gaps closed | 🔴 Pending |
| Sprint 4 Complete | [Week 5 end] | Data model complete | 🔴 Pending |
| Sprint 5 Complete | [Week 6 end] | All gaps closed | 🔴 Pending |

---

## Resource Allocation

| Role | Allocation | Sprints | Total Hours |
|------|------------|---------|-------------|
| Senior Developer | 50% | 1-2 | 50h (API docs) |
| Technical Writer | 100% | 3-4 | 65h (User guides, polish) |
| Architect | 25% | 1, 4 | 30h (Architecture, data model) |

---

## Risk Management

### Risks

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Resources unavailable | HIGH | MEDIUM | Cross-train, allocate backup resources |
| Scope creep (new gaps discovered) | MEDIUM | HIGH | Timebox sprints, move to backlog |
| Documentation accuracy issues | HIGH | MEDIUM | Source code verification protocol |
| Tool/process changes during work | LOW | LOW | Version lock, change control |

### Contingency

- **If behind schedule**: Drop LOW priority items to backlog
- **If ahead of schedule**: Pull MEDIUM items forward
- **If resources unavailable**: Reschedule non-CRITICAL work

---

## Success Metrics

**Target Metrics** (by end of Sprint 5):
- ✅ Documentation coverage > 85%
- ✅ All CRITICAL gaps closed
- ✅ All HIGH gaps closed
- ✅ No security documentation gaps
- ✅ API coverage > 80%
- ✅ User guide coverage > 70%

**Validation**:
- Run gap analysis again after Sprint 5
- Compare before/after metrics
- Validate with stakeholders
```

---

## Phase 6: Execute Remediation

### 6.1 Work Each Sprint

**For Each Sprint**:

1. **Review Sprint Plan**
   ```bash
   code .gap-analysis/remediation-plan.md
   ```

2. **For Each Task**:
   - Review gap details
   - Review source code (use SOURCE-CODE-VERIFICATION-PROTOCOL)
   - Create/update documentation
   - Add source code references
   - Add examples
   - Validate accuracy
   - Commit changes

3. **Track Progress**:
   - Update task checklist in remediation plan
   - Update milestone status
   - Update coverage metrics

### 6.2 Document Progress

**Daily/Weekly Updates**:

Create `.gap-analysis/progress-log.md`:

```markdown
# Gap Remediation Progress Log

## Week 1 (Sprint 1)

### Day 1 - [Date]
**Completed**:
- ✅ Updated `docs/design/06-authentication-security.md`
  - Added password policy section
  - Documented session management
  - Updated encryption algorithms
  - Verified against source code: `/src/Auth/PasswordHasher.cs:45`

**Time Spent**: 4 hours
**Issues**: None
**Next**: PatientService API documentation

### Day 2 - [Date]
**Completed**:
- ✅ Created `docs/api/patient-service.md`
  - Documented 12 public methods
  - Added usage examples for core methods
  - Added error handling documentation

**Time Spent**: 6 hours
**Issues**: Found 2 undocumented methods, added to backlog
**Next**: Patient entity documentation

[Continue daily/weekly...]
```

---

## Phase 7: Validate Gap Closure

### 7.1 Re-Run Gap Analysis

**Purpose**: Measure improvement

```bash
# Re-run all gap identification steps from Phase 2
# Compare new metrics to baseline

# Calculate improvement
echo "Before: 52% coverage"
echo "After: XX% coverage"
echo "Improvement: +XX%"
```

### 7.2 Create Completion Report

Create `.gap-analysis/completion-report.md`:

```markdown
# Documentation Gap Remediation - Completion Report

**Remediation Period**: [start-date] to [end-date]
**Duration**: [X] weeks
**Total Effort**: [X] hours (Planned: [Y] hours)

---

## Summary

**Before Remediation**:
- Total Items: 168
- Documented: 88
- Undocumented: 80
- Coverage: 52%

**After Remediation**:
- Total Items: 168
- Documented: 155
- Undocumented: 13
- Coverage: 92%

**Improvement**: +40% coverage

---

## Gaps Closed by Category

| Category | Before | After | Improvement | Status |
|----------|--------|-------|-------------|--------|
| Architecture | 80% | 100% | +20% | ✅ Complete |
| APIs | 40% | 85% | +45% | ✅ Target met |
| Data Model | 63% | 95% | +32% | ✅ Target met |
| User Guides | 33% | 80% | +47% | ✅ Target met |
| Configuration | 50% | 85% | +35% | ✅ Target met |
| Security | 75% | 100% | +25% | ✅ Complete |
| Deployment | 80% | 100% | +20% | ✅ Complete |
| Testing | 67% | 85% | +18% | ✅ Target met |

---

## Gaps Closed by Priority

| Priority | Planned | Completed | % Complete |
|----------|---------|-----------|------------|
| CRITICAL | 15 | 15 | 100% ✅ |
| HIGH | 35 | 32 | 91% ✅ |
| MEDIUM | 20 | 18 | 90% ✅ |
| LOW | 10 | 2 | 20% 🔴 |

**Note**: LOW priority items deferred to backlog by agreement

---

## Documentation Created/Updated

### New Documentation Files
- `docs/api/patient-service.md` - PatientService API
- `docs/api/stream-service.md` - StreamService API
- `docs/user-guide/stream-recording.md` - Stream recording workflow
- [List all new files]

**Total New Files**: [count]

### Updated Documentation Files
- `docs/design/05-data-model.md` - Added 12 entities
- `docs/design/06-authentication-security.md` - Updated security sections
- [List all updated files]

**Total Updated Files**: [count]

---

## Effort Analysis

**Planned Effort**: 145 hours
**Actual Effort**: [X] hours
**Variance**: [+/-X] hours ([+/-X]%)

**Breakdown**:
- Sprint 1 (CRITICAL): Planned 30h, Actual [X]h
- Sprint 2 (HIGH APIs): Planned 35h, Actual [X]h
- Sprint 3 (User Guides): Planned 40h, Actual [X]h
- Sprint 4 (Data Model): Planned 25h, Actual [X]h
- Sprint 5 (Remaining): Planned 15h, Actual [X]h

---

## Remaining Gaps (Backlog)

**Count**: 13 items (LOW priority)
**Estimated Effort**: 20 hours

| Item | Type | Priority | Effort | Target Date |
|------|------|----------|--------|-------------|
| [Item] | [Type] | LOW | [X]h | Backlog |

---

## Success Criteria Met

- ✅ Documentation coverage > 85% (Achieved: 92%)
- ✅ All CRITICAL gaps closed (15/15)
- ✅ All HIGH gaps closed (32/35, 91%)
- ✅ No security documentation gaps (100%)
- ✅ API coverage > 80% (Achieved: 85%)
- ✅ User guide coverage > 70% (Achieved: 80%)

---

## Lessons Learned

### What Went Well
- Systematic gap identification process
- Clear prioritization framework
- Regular progress tracking
- Source code verification prevented errors

### What Could Improve
- Initial time estimates were optimistic for user guides
- Should have allocated more time for screenshots
- Need better tooling for maintaining diagrams

### Recommendations for Future
- Run gap analysis quarterly
- Integrate gap checks into PR review process
- Create documentation templates to speed creation
- Automate coverage metric calculation

---

## Next Steps

1. **Immediate**:
   - [ ] Archive gap analysis files
   - [ ] Update CLAUDE.md with gap analysis session
   - [ ] Notify stakeholders of completion

2. **Short-Term** (Next Sprint):
   - [ ] Complete remaining 3 HIGH priority gaps
   - [ ] Review and groom backlog

3. **Long-Term** (Quarterly):
   - [ ] Schedule next gap analysis
   - [ ] Implement continuous documentation coverage monitoring
   - [ ] Create documentation maintenance protocol

---

**Report Completed**: [YYYY-MM-DD]
**Next Gap Analysis**: [date + 3 months]
**Owner**: [Name]
```

---

## Integration with Other Protocols

### Source Code Verification Integration
- After closing gaps, verify accuracy with SOURCE-CODE-VERIFICATION-PROTOCOL
- Ensure all documentation matches actual code

### PR Documentation Integration
- When reviewing PRs, check for documentation gaps
- Use gap analysis metrics to track coverage over time

---

## Success Metrics

This protocol is successful when:

- ✅ **Gaps Identified**: All documentation gaps cataloged
- ✅ **Metrics Calculated**: Coverage percentage known
- ✅ **Plan Created**: Remediation plan with sprints
- ✅ **Gaps Closed**: Target coverage achieved (typically 85%+)
- ✅ **Validated**: Re-analysis confirms improvement

---

## Appendix: Quick Reference

```bash
# Phase 1: Define Scope
mkdir -p .gap-analysis
[Define documentation standards]

# Phase 2: Identify Gaps
find docs/ -name "*.md" | sort > .gap-analysis/current-docs.txt
[Compare code to docs, identify gaps]

# Phase 3: Measure Coverage
[Calculate percentages, create metrics]

# Phase 4: Prioritize
[Apply prioritization framework]

# Phase 5: Plan
[Create sprint-based remediation plan]

# Phase 6: Execute
[Work sprints, document progress]

# Phase 7: Validate
[Re-run analysis, create completion report]
```

---

**For AI Assistants**: This protocol should be executed when:
1. User requests "documentation gap analysis"
2. Starting documentation for existing codebase
3. Preparing for audit/release
4. Periodic documentation quality review

Use TodoWrite tool to track progress through all 7 phases.

**Typical Execution Time**: 4-16 hours for analysis, 40-200 hours for remediation (varies by gap count)
