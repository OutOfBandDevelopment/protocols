# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is an **architectural documentation wrapper** for Cadwell Industries' medical device software portfolio. It aggregates 14 separate Git repositories as submodules to provide a unified view for understanding interconnections and dependencies across the product ecosystem.

---

## ⚠️ CRITICAL RULES - READ FIRST ⚠️

### 1. NEVER CREATE FILES WITHIN SUBMODULES

**ABSOLUTE RULE:** Do NOT create, edit, or delete ANY files within submodule directories (Cascade/, Arc/, CadLink/, etc.)

**Correct Location for Documentation:**
- ✅ `.\docs\Cascade\` - Correct (outer AllApps project)
- ✅ `.\docs\Arc\` - Correct (outer AllApps project)
- ✅ `.\docs\shared\` - Correct (outer AllApps project)
- ❌ `Cascade\docs\` - WRONG (inside submodule)
- ❌ `Arc\docs\` - WRONG (inside submodule)
- ❌ `CadLink\anything.md` - WRONG (inside submodule)

**Why This Matters:**
- Each submodule is an independent Git repository
- Changes in submodules belong in their respective repositories
- This repository is ONLY for architectural documentation and aggregation

**If Files Were Created in Wrong Location:**
1. Move them to `.\docs\<ProjectName>\` in the outer project
2. Remove them from the submodule using `git clean -fd <path>`
3. Verify submodule is clean with `git status` inside the submodule

### 2. DO NOT USE PYTHON

This project does NOT have Python available. Use .NET or Node.js for any tooling.

---

## Repository Structure

All directories (except `LegacyProjects/`) are Git submodules pointing to individual Azure DevOps repositories:

| Submodule | Purpose | Technology |
|-----------|---------|------------|
| **CadLink** | Legacy data management platform for IONM and patient records | C#/.NET, WPF, SQLite/SQL Server |
| **Arc** | Intraoperative neuromonitoring (IONM) application | C#/.NET, WPF, Medical Device Software |
| **Cascade** | Modern surgical studio for IONM with multi-modal monitoring | C#/.NET 4.8, WPF, DevExpress |
| **Hypnos** | Sleep medicine recording and analysis system | C#/.NET 4.8, WPF, CadLink integration |
| **Sierra** | Foundational library with CPIN peripherals and legacy components | C#/.NET, WPF, C++/native code |
| **CloudEEG** | Cloud-based EEG viewing and analysis platform | ASP.NET Core, React/TypeScript |
| **Fleet** | Enterprise asset management and operations platform | ASP.NET Core, .NET Aspire, Wolverine |
| **Core** | Shared component library (submodule used by Arc, Hypnos, Cascade) | C#/.NET, WPF utilities |
| **UIAutomationCore** | Test automation framework with PageObject model | C#/.NET, MSTest, FlaUI |
| **Arcade** | Infrastructure and tools shared across teams | C#/.NET utilities |
| **ProductQuality** | QA tools, test documentation, Azure DevOps integration | Python, Claude Code configs |
| **APISamples** | Example code and sample integrations | C#/.NET examples |
| **CadLinkDbFixer** | Database maintenance and migration utility | C#/.NET, SQLite, SQL Server |
| **WebResources** | Web-based resources and documentation | HTML/CSS/JS |

## Working with Submodules

### Common Git Submodule Commands

```bash
# Initialize and clone all submodules after initial repository clone
git submodule update --init --recursive

# Update all submodules to their latest commits on tracked branches
git submodule update --remote --recursive

# Check status of all submodules
git submodule status

# Update a specific submodule to latest
git submodule update --remote <submodule-name>

# Pull latest changes in all submodules
git submodule foreach git pull origin main

# View which commit each submodule is pointing to
git submodule
```

### Updating Submodule References

When a submodule has been updated in its own repository and you want to update the pointer in AllApps:

```bash
# Navigate to the submodule
cd <submodule-name>

# Pull latest changes
git pull origin main

# Return to AllApps root
cd ..

# Stage the updated submodule reference
git add <submodule-name>

# Commit the updated reference
git commit -m "Update <submodule-name> to latest"
```

## Medical Device Software Context

This repository aggregates **safety-critical medical device software** used for:
- **Intraoperative Neuromonitoring (IONM)**: Real-time monitoring during surgery (Arc, Cascade)
- **Sleep Medicine**: EEG recording and analysis for sleep studies (Hypnos)
- **Enterprise Operations**: Device management, personnel tracking (Fleet)
- **Data Management**: Patient records, case data, regulatory compliance (CadLink)

### Critical Characteristics
- FDA-regulated medical device software
- Real-time performance requirements (waveform processing)
- Patient Health Information (PHI) protection
- Multi-generational hardware support (Cascade16/Pro/Elite, IOMAX, Guardian)
- Monitoring modalities: SSEP, VEP, BAEP, MEP, TEMG, EEG, DCS, Threshold, TOF

## Technology Stack Overview

- **Primary Language**: C# with .NET Framework 4.8 and modern .NET
- **Desktop UI**: WPF with DevExpress controls
- **Data Access**: Entity Framework Core, SQLite, SQL Server
- **Cloud Services**: ASP.NET Core, .NET Aspire, Wolverine message bus
- **Web Frontend**: React/TypeScript (CloudEEG)
- **Testing**: MSTest, UIAutomationCore framework
- **Build System**: MSBuild, Azure Pipelines
- **Source Control**: Azure DevOps Git repositories
- **Package Management**: NuGet with internal Azure DevOps feeds

## Project-Specific Documentation

Several submodules have their own CLAUDE.md files with detailed context:

- **Cascade/CLAUDE.md**: Surgical studio architecture, domain models, monitoring modes
- **Fleet/CLAUDE.md**: Enterprise software patterns, module commands, database operations
- **UIAutomationCore/CLAUDE.md**: Test framework architecture, PageObject patterns

When working on a specific project, navigate to that submodule's repository and consult its documentation.

## Documentation Framework

This repository provides **technical documentation for v-next planning and impact analysis**:

### Documentation Purpose

**Primary Goal:** Support CadLink v-next redesign by understanding:
1. What CadLink APIs each application uses
2. How they use them (sync patterns, auth, offline behavior)
3. What breaks if CadLink changes
4. What Core library features are used and where
5. What patterns/functionality overlaps across applications

**NOT for:** Business value, ROI, marketing, or hypothetical personas

### Documentation Structure

```
docs/<ProjectName>/
├── README.md                       # Project overview with navigation
├── glossary.md                     # Project-specific terminology
├── technical/                      # Technical architecture
│   ├── system-architecture.md      # C4 diagrams, components, deployment
│   ├── technology-stack.md         # Exact versions, NuGet packages
│   ├── data-architecture.md        # Database schema, EF models
│   ├── communication-protocols.md  # Binary protocols, message formats
│   └── data-schemas.md             # JSON/XML schemas, contracts
├── patterns/                       # Actual design patterns used WITH CODE REFS
│   ├── architectural/              # High-level patterns
│   ├── design/                     # Code-level patterns
│   └── domain/                     # Domain-specific patterns
├── features/                       # What the app DOES (not "business")
│   ├── README.md                   # Feature catalog
│   ├── feature-matrix.md           # Complete feature grid
│   └── <feature-name>.md           # Full stack trace: UI→VM→Domain→Repo→DB→Integration
├── integrations/                   # **CRITICAL** External system deep dives
│   ├── cadlink-integration.md      # All CadLink APIs used, sync patterns, auth
│   ├── cloudeeg-integration.md     # CloudEEG integration
│   ├── core-library-usage.md       # What Core features used, where, how
│   └── hardware-integration.md     # CPIN/hardware protocols
├── data/                           # Complete data architecture
│   ├── database-schema.md          # Every table, field, FK, index
│   ├── data-flow-diagrams.md       # UI field → VM → Domain → DB mappings
│   ├── entity-relationship.md      # ER diagrams (PlantUML)
│   └── data-sync-patterns.md       # How data moves between systems
├── security/                       # Actual implementation (NOT compliance claims)
│   ├── authentication.md           # Actual auth code with references
│   ├── authorization.md            # Actual RBAC implementation
│   ├── data-protection.md          # Actual encryption code
│   └── audit-logging.md            # Actual audit implementation
└── ui/                             # UI structure and patterns
    ├── ui-architecture.md          # MVVM implementation details
    ├── screen-catalog.md           # Every screen with SALT diagrams
    └── ui-data-binding.md          # XAML bindings to ViewModels
```

### Shared Documentation (Cross-Project Analysis)

```
docs/shared/
├── dependency-matrix.md            # What depends on what
├── framework-usage-matrix.md       # Core library usage across apps
├── integration-patterns.md         # Common integration approaches
├── cadlink-impact-analysis.md      # All CadLink usages across all apps
└── overlap-analysis.md             # Shared functionality analysis
```

### Documentation Protocols

Comprehensive protocols in `.claude/protocols/`:
- **documentation-style-guide.md** - Standards for all documentation
- **architectural-analysis.md** - Systematic analysis procedure
- **comprehensive-analysis-checklist.md** - Completeness checklist
- **protocol-validation.md** - Protocol quality assurance

## Common Use Cases for This Repository

### 1. V-Next CadLink Planning (Primary Use Case)

**Question:** "I'm redesigning CadLink - what will break?"

**Steps:**
1. Start with `docs/shared/cadlink-impact-analysis.md` for overall impact
2. For each consuming app (Cascade, Arc, Hypnos):
   - Read `docs/<App>/integrations/cadlink-integration.md` for:
     - Every API endpoint used
     - Authentication mechanism
     - Sync frequency and patterns
     - Offline queue behavior
     - Data mapping (what fields are synced)
3. Check `docs/<App>/data/data-sync-patterns.md` for data migration needs
4. Review code references to understand actual implementation

### 2. Core Library Changes

**Question:** "If I change Core library, what breaks?"

**Steps:**
1. Check `docs/shared/framework-usage-matrix.md` for all usages
2. For each consuming app:
   - Read `docs/<App>/integrations/core-library-usage.md`
   - See exact UI controls, services, utilities used
   - Find file:line references for each usage
3. Assess migration effort per application

### 3. Understanding Application Implementation

**Question:** "How does Cascade implement feature X?"

**Steps:**
1. Start with `docs/Cascade/features/<feature-name>.md` for full stack trace:
   - UI → ViewModel → Domain → Repository → Database → Integration
2. Follow code references to actual implementation
3. Check `docs/Cascade/patterns/` for design patterns used
4. Review `docs/Cascade/data/data-flow-diagrams.md` for data mapping

### 4. Finding Overlap Between Applications

**Question:** "Do Arc and Cascade share similar implementations?"

**Steps:**
1. Review `docs/shared/overlap-analysis.md`
2. Compare pattern usage in `docs/Arc/patterns/` vs `docs/Cascade/patterns/`
3. Check if both use same Core library features
4. Identify opportunities for shared library extraction

### 5. Database Schema Understanding

**Question:** "What's the complete database structure?"

**Steps:**
1. Read `docs/<App>/data/database-schema.md` for:
   - Every table with SQL CREATE statements
   - All fields, FKs, indexes
   - Entity Framework mappings with code references
2. Review `docs/<App>/data/entity-relationship.md` for ER diagrams
3. Check `docs/<App>/data/data-flow-diagrams.md` for field-level mappings
- Review data flows in `data/data-flow.md` for data-dependent changes

## Documentation Working Conventions

### Tools and Scripting

**IMPORTANT: No Python**
- Python is NOT available in this environment
- Use **.NET (C#)** or **Node.js** for tools and utilities
- Tool location: `.\tools\{toolname}\`
- Script location: `.\scripts\{scriptname}\`

**Scripting Preferences:**
- **Prefer**: PowerShell (.ps1) and Batch files (.bat)
- **Avoid**: Bash scripts (use PowerShell equivalents)

**Examples:**
```
.\tools\
  └── doc-generator\       # .NET console app or Node.js script
.\scripts\
  └── analyze-project.ps1  # PowerShell script
  └── build-docs.bat       # Batch file
```

### Work Tracking

**Root-Level TODO**: `.\TODO.md`
- Track ongoing work across all projects
- Reference specific project tasks
- Update as work progresses

**Change History**: `.\docs\changes\{project}\{area}-{topic}.md`
- Document what changed, when, and why
- Organized by project and topic area
- Examples:
  - `.\docs\changes\Cascade\technical-architecture.md`
  - `.\docs\changes\Cascade\patterns-mvvm.md`
  - `.\docs\changes\shared\common-patterns.md`

### Documentation Requirements

**Every Document Must Include:**
1. **Table of Contents** - For documents >100 lines
2. **Submodule Reference** - Branch and commit hash when referencing code
3. **Source Code Links** - Link to files using relative paths (e.g., `[File.cs](../../Cascade/src/File.cs)`)
4. **Last Updated Date** - YYYY-MM-DD format
5. **Change Log** - At bottom of document

**Specialized Documentation:**

**Glossaries** (`glossary.md` per project)
- Medical/clinical terms
- Technical terms
- Business terms
- Acronyms and abbreviations

**UI Pattern Analysis** (in `users/ui-structure.md`)
- Document UI pattern used (MVVM, MVC, code-behind)
- Show ViewModel → UI element → Domain → Database mappings
- Include validation rules and implementation (INotifyDataErrorInfo, etc.)
- Use PlantUML SALT for UI layouts

**System Diagrams Required:**
- **Deployment Diagrams** - Show physical/logical deployment
- **ER Diagrams** - Entity relationships for data models
- **Data Flow Diagrams** - UI → ViewModel → Domain → Database with field mappings
- All diagrams embedded in markdown (PlantUML)

### CRITICAL: No Unverified Claims

**ABSOLUTE RULE: Never include data without a verifiable source**

**Prohibited without source citation:**
- ROI calculations, cost savings, time savings
- Performance metrics not from code/tests/monitoring
- Workflow durations not from observation/measurement
- Market data, competitive claims
- User satisfaction or business value claims

**Every claim must have:**
```markdown
Statement here.
**Source:** [Description with link/reference] (Date)
```

**For business/user flows:**
```markdown
## Workflow Name
**Process Source:** [Where/how determined]
**Documented By:** [Name/role]
**Last Verified:** [Date]
```

**If no source exists:**
- Mark as "**Assumption:**" or "**Hypothetical:**"
- Or leave as "**TO BE DOCUMENTED**"
- NEVER make up data

See [Documentation Style Guide](`.claude/protocols/documentation-style-guide.md`) for full validation rules.

### Code References

When documenting code:
```markdown
The plugin discovery happens in [PluginManager.cs](../../Cascade/src/Core/PluginManager.cs:45-120)
(Cascade @ master `dfec8f78`)
```

Include:
- Relative link to source file
- Line numbers if specific section
- Submodule name, branch, and short commit hash

## Development Workflow Notes

**For Active Development**: Clone and work in individual project repositories, not in AllApps submodule directories.

**For AllApps Repository**:
- Update submodule references when projects release new versions
- Document architectural decisions that span multiple projects
- Maintain this CLAUDE.md and other cross-project documentation

## Azure DevOps Integration

All submodules are hosted in the Azure DevOps organization "Cadwell" under the "Engr" project:
- Repository URLs: `https://Cadwell@dev.azure.com/Cadwell/Engr/_git/<ProjectName>`
- CI/CD pipelines configured per project
- Internal NuGet feeds for package management
- Wiki documentation for development guidelines

## Key Architectural Insights

### Shared Core Pattern
Arc, Cascade, and Hypnos all reference Core as a Git submodule, creating a shared foundation for:
- UI components and controls
- Media handling and waveform rendering
- Licensing and diagnostics
- Common utilities

### Medical Device Architecture
- **Hardware Abstraction**: Multiple hardware platforms with unified interfaces (CPIN)
- **Real-Time Processing**: Waveform display and signal processing pipelines
- **Domain Separation**: Domain Models vs ViewModels in Cascade/Hypnos
- **Data Interfaces**: IReadonlyProcedure and IRecordingProcedure patterns

### Enterprise Integration
- Fleet uses modern .NET Aspire for cloud-native architecture
- CadLink provides central data repository for patient/case data
- CloudEEG enables remote review and collaboration
- ProductQuality integrates Azure DevOps for test management
