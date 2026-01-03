# Architectural Analysis Protocol

**Version:** 1.0
**Last Updated:** 2026-01-02
**Purpose:** Standardized procedure for analyzing and documenting submodule architecture in the AllApps repository.

## Overview

This protocol guides the systematic analysis of each submodule's architecture, ensuring consistent documentation, pattern identification, and cross-project insights. It produces structured documentation following the style guide while capturing both technical and organizational details.

## When to Use

- Initial architectural analysis of a submodule
- Periodic architecture reviews (quarterly recommended)
- After major architectural changes in a submodule
- When onboarding new team members
- When identifying cross-project patterns

## Prerequisites

- [ ] Access to AllApps repository with submodules initialized
- [ ] Read access to submodule source code
- [ ] [Documentation Style Guide Protocol](./documentation-style-guide.md) reviewed
- [ ] PlantUML rendering tools available
- [ ] Git command-line access

## Procedure

### Step 1: Capture Submodule Metadata

Record current state of the submodule for version tracking.

```bash
# Navigate to submodule
cd <SubmoduleName>

# Capture metadata
echo "Submodule: $(basename $(pwd))"
echo "Branch: $(git rev-parse --abbrev-ref HEAD)"
echo "Commit: $(git rev-parse HEAD)"
echo "Short Hash: $(git rev-parse --short HEAD)"
echo "Last Commit Date: $(git log -1 --format=%cd --date=short)"
echo "Last Commit Author: $(git log -1 --format='%an <%ae>')"

# Return to AllApps root
cd ..
```

**Validation:**
- [ ] Branch name captured
- [ ] Full commit hash captured
- [ ] Short hash (7 chars) captured
- [ ] Last commit date captured

### Step 2: Create Project Documentation Directory

Create documentation directory for the submodule.

```bash
# From AllApps root
# Create project directory (match submodule name exactly)
mkdir -p docs/<SubmoduleName>

# Create standard documentation files
touch docs/<SubmoduleName>/architecture.md
touch docs/<SubmoduleName>/patterns.md
touch docs/<SubmoduleName>/README.md

# Example for Cascade:
mkdir -p docs/Cascade
touch docs/Cascade/architecture.md
touch docs/Cascade/patterns.md
touch docs/Cascade/README.md
```

**Validation:**
- [ ] Project directory created under `docs/`
- [ ] Directory name matches submodule name exactly
- [ ] Three standard files created (architecture.md, patterns.md, README.md)

### Step 3: Create Shared Documentation Structure (First Time Only)

If this is the first project being analyzed, create shared documentation structure.

```bash
# From AllApps root
mkdir -p docs/shared

# Create shared documentation files
touch docs/shared/common-patterns.md
touch docs/shared/technology-stack.md
touch docs/shared/dependency-map.md
touch docs/shared/integration-points.md
touch docs/shared/system-context.md
touch docs/shared/README.md
```

**Validation:**
- [ ] `docs/shared/` directory exists
- [ ] Core shared documentation files created

### Step 4: Initial Discovery - Project Structure

Explore the submodule to understand its organization.

**3.1: Identify Build System**

```bash
cd <SubmoduleName>

# Check for .NET solutions
find . -name "*.sln" -type f

# Check for project files
find . -name "*.csproj" -type f | wc -l

# Check for package management
ls -la | grep -E "(packages.config|Directory.Packages.props|paket)"

# Check for other build systems
ls -la | grep -E "(package.json|build.gradle|CMakeLists.txt|Makefile)"
```

**3.2: Identify Key Directories**

```bash
# List top-level structure
ls -la

# Identify common patterns
ls -la src/ 2>/dev/null || echo "No src/ directory"
ls -la test/ 2>/dev/null || echo "No test/ directory"
ls -la tests/ 2>/dev/null || echo "No tests/ directory"
ls -la docs/ 2>/dev/null || echo "No docs/ directory"
```

**3.3: Identify Documentation**

```bash
# Find existing documentation
find . -name "README.md" -o -name "CLAUDE.md" -o -name "*.md" | head -20

# Check for Cursor/Copilot rules
find . -path "*/.cursor/*" -o -name ".cursorrules" -o -path "*/.github/copilot-instructions.md"
```

**Validation:**
- [ ] Build system identified
- [ ] Project count estimated
- [ ] Directory structure mapped
- [ ] Existing documentation cataloged

### Step 5: Analyze Architecture

Perform systematic architectural analysis.

**4.1: Technology Stack**

Document in `technologies.md`:

```markdown
# Technologies - [SubmoduleName]

**Last Updated:** YYYY-MM-DD
**Submodule References:**
  - `<SubmoduleName>` @ `<branch>` (`<commit-hash>`)

## Programming Languages
- [Language] [Version]

## Frameworks
- [Framework] [Version]

## Key Libraries
| Library | Version | Purpose |
|---------|---------|---------|
| [Name] | [Ver] | [Purpose] |

## Build Tools
- [Tool] [Version]

## Testing Frameworks
- [Framework] [Version]

## Data Storage
- [Database/Format] [Version]

## UI Framework
- [Framework] [Version]
```

**4.2: System Architecture**

Identify architectural layers and components:

- Presentation layer (UI)
- Application layer (business logic)
- Domain layer (domain models)
- Infrastructure layer (data access, external systems)

**4.3: Key Design Decisions**

Look for:
- Dependency injection setup
- Configuration management
- Error handling strategies
- Logging approach
- Testing strategy

**Validation:**
- [ ] Technology stack documented
- [ ] Architectural layers identified
- [ ] Key libraries cataloged
- [ ] Design decisions noted

### Step 6: Create C4 Context Diagram (Embedded)

Add C4 Context diagram directly in the architecture markdown file.

**Add to architecture file:**

````markdown
## System Context

```plantuml
@startuml
'title: C4 Context Diagram - [SubmoduleName]
'version: 1.0
'date: YYYY-MM-DD
'submodule: <SubmoduleName> @ <branch> (<commit-hash>)

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

LAYOUT_WITH_LEGEND()

'Define primary actors/users
Person(user, "[User Role]", "[Description]")

'Define this system
System(system, "[SubmoduleName]", "[Purpose]")

'Define external systems
System_Ext(external1, "[External System]", "[Description]")
SystemDb_Ext(db, "[Database]", "[Type]")

'Define relationships
Rel(user, system, "[Action]", "[Protocol/Tech]")
Rel(system, external1, "[Action]", "[Protocol/Tech]")
Rel(system, db, "[Action]", "[Protocol/Tech]")

@enduml
```

### Context Explanation

[Describe the external dependencies, actors, and how this system fits into the broader ecosystem]

**External Dependencies:**
- **[System Name]**: [Purpose of integration]
- **[Database]**: [What data is stored/retrieved]

**Key Integration Points:**
- [Integration point 1]
- [Integration point 2]
````

**Validation:**
- [ ] Diagram embedded in architecture file
- [ ] Diagram includes metadata comments (version, date, submodule reference)
- [ ] All external dependencies identified
- [ ] User actors defined
- [ ] Relationships labeled with protocols/technologies
- [ ] Context explanation provided
- [ ] Diagram syntax is valid (test with PlantUML renderer)

### Step 7: Create C4 Container Diagram (Embedded)

Add C4 Container diagram directly in the architecture markdown file.

**Add to architecture file:**

````markdown
## Container Architecture

```plantuml
@startuml
'title: C4 Container Diagram - [SubmoduleName]
'version: 1.0
'date: YYYY-MM-DD
'submodule: <SubmoduleName> @ <branch> (<commit-hash>)

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

Person(user, "[User Role]", "[Description]")

System_Boundary(c1, "[SubmoduleName]") {
  Container(app, "[Application]", "[Tech Stack]", "[Description]")
  ContainerDb(db, "[Database]", "[Technology]", "[Description]")
  Container(service, "[Background Service]", "[Tech]", "[Description]")
}

System_Ext(external, "[External System]", "[Description]")

Rel(user, app, "[Action]")
Rel(app, db, "[Action]", "[Protocol]")
Rel(app, service, "[Action]", "[Protocol]")
Rel(service, external, "[Action]", "[Protocol]")

@enduml
```

### Container Explanation

[Describe the major containers, their responsibilities, and how they interact]

**Containers:**
- **[Application]**: [Purpose, technology, key responsibilities]
- **[Database]**: [Storage technology, data model]
- **[Service]**: [Background processing, scheduled tasks, etc.]
````

**Validation:**
- [ ] Diagram embedded in architecture file
- [ ] Major containers identified (applications, services, databases)
- [ ] Technology stack labeled for each container
- [ ] Inter-container relationships documented
- [ ] Container explanations provided
- [ ] Diagram syntax is valid

### Step 8: Identify and Document Patterns

Analyze code for patterns and practices.

**7.1: Code Organization Patterns**

Search for:
```bash
# Check for common folder structures
find . -type d -name "Models" -o -name "ViewModels" -o -name "Views" -o -name "Services" | head -20

# Look for architectural patterns
grep -r "interface I" --include="*.cs" | grep -E "(Repository|Service|Factory|Strategy)" | head -10

# Check for dependency injection
grep -r "IServiceCollection" --include="*.cs" | head -5
```

**7.2: Design Patterns**

Look for evidence of:
- Repository pattern
- Factory pattern
- Strategy pattern
- Observer/Event patterns
- MVVM pattern (for WPF apps)
- Dependency Injection

**7.3: Testing Patterns**

```bash
# Find test projects
find . -name "*.Test*.csproj" -o -name "*.Tests.csproj"

# Check testing framework
grep -r "Microsoft.VisualStudio.TestTools.UnitTesting" --include="*.cs" | head -1
grep -r "using Xunit" --include="*.cs" | head -1
grep -r "using NUnit" --include="*.cs" | head -1

# Look for test organization
find . -path "*/Tests/*" -name "*.cs" | head -10
```

Document findings in `patterns.md`:

```markdown
# Patterns and Practices - [SubmoduleName]

**Last Updated:** YYYY-MM-DD
**Submodule References:**
  - `<SubmoduleName>` @ `<branch>` (`<commit-hash>`)

## Pattern Name: [Pattern Name]

**Category:** [Architectural | Design | Code Organization | Testing]
**Confidence:** [High | Medium | Low]
**Evidence:** [File references]

### Intent
[What problem does this solve?]

### Implementation
[How is it implemented? Include code references with line numbers]

### Example
```language
[Code snippet with file reference]
```

### Benefits
- [Benefit 1]

### Drawbacks
- [Drawback 1]

### Related Patterns
- [Pattern name]
```

**Validation:**
- [ ] At least 3 patterns identified and documented
- [ ] Code examples include file references with submodule version
- [ ] Confidence level assessed
- [ ] Evidence provided

### Step 9: Complete Architecture Documentation

Fill out the complete architecture file with all sections.

**Complete Template Structure:**

```markdown
# [SubmoduleName] Architecture

**Last Updated:** YYYY-MM-DD
**Status:** Active
**Topic:** [Topic] (e.g., medical-devices, enterprise, infrastructure)
**Epic:** [Epic] (e.g., ionm-systems, data-management)
**Submodule References:**
  - `<SubmoduleName>` @ `<branch>` (`<commit-hash>`)

## Overview
[2-3 sentence system overview - purpose, domain, key capabilities]

## System Context

```plantuml
@startuml
'title: C4 Context Diagram - [SubmoduleName]
'version: 1.0
'date: YYYY-MM-DD
'submodule: <SubmoduleName> @ <branch> (<commit-hash>)

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml
' ... diagram content ...
@enduml
```

[Explanation of external dependencies and actors]

## Container Architecture

```plantuml
@startuml
'title: C4 Container Diagram - [SubmoduleName]
'version: 1.0
'date: YYYY-MM-DD
'submodule: <SubmoduleName> @ <branch> (<commit-hash>)

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
' ... diagram content ...
@enduml
```

[Explanation of major containers and their responsibilities]

## Component Breakdown

### [Component Name]
**Location:** `path/to/component`
**Purpose:** [Purpose]
**Key Classes:**
- `ClassName` (path/to/file.cs:line) - [Description]

**Dependencies:**
- [Dependency 1]

## Data Architecture

### Data Models
[Domain model description]

### Data Flow
[How data moves through the system]

### Storage
[Database, files, etc.]

## Key Design Decisions

### Decision: [Title]
**Date:** YYYY-MM-DD
**Status:** [Accepted | Superseded | Deprecated]

**Context:**
[What is the issue we're trying to solve?]

**Decision:**
[What did we decide?]

**Consequences:**
[What are the trade-offs?]

## Integration Points

### [External System Name]
**Type:** [Database | Service | Library | Hardware]
**Protocol:** [HTTP, gRPC, Direct, etc.]
**Location:** [Where is integration code?]

## Testing Architecture

### Unit Tests
**Location:** [Path]
**Framework:** [Framework]
**Coverage:** [Estimate if available]

### Integration Tests
**Location:** [Path]
**Framework:** [Framework]

## Build and Deployment

### Build Process
[How to build]

### Dependencies
[Key dependencies]

### Deployment
[How is it deployed?]

## Related Documentation
- [Link to patterns.md]
- [Link to technologies.md]
- [Link to submodule CLAUDE.md if exists]

## Change Log
- YYYY-MM-DD v1.0: Initial architecture documentation
```

**Validation:**
- [ ] All sections completed
- [ ] Diagrams embedded and rendering
- [ ] Component breakdown includes file references
- [ ] Integration points documented
- [ ] Build process documented

### Step 10: Document Cross-Cutting Concerns

After analyzing the project, identify cross-cutting patterns and dependencies.

**10.1: Update Cross-Cutting Patterns**

Edit `docs/shared/common-patterns.md`:

```markdown
## Pattern: [Pattern Name]

**Used In:** [List of projects, e.g., "Cascade, Arc, Hypnos"]
**Category:** [Architectural | Design | Code Organization | Testing]
**Confidence:** [High | Medium | Low]

### Intent
[What problem does this solve?]

### Implementations

#### In [Project1]
[How it's implemented in this project]
- Reference: `path/to/file.cs:line` ([Project1] @ branch `hash`)

#### In [Project2]
[How it's implemented in this project - note variations]
- Reference: `path/to/file.cs:line` ([Project2] @ branch `hash`)

### Variations
[Document differences in implementation across projects]

### Evolution
[How this pattern has evolved across projects/time]
```

**10.2: Update Dependency Map**

Edit `docs/shared/dependency-map.md`:

```markdown
### [SubmoduleName] Dependencies

**Depends On:**
- **[Dependency]**: [Purpose of dependency]
  - Integration method: [NuGet, Git submodule, Direct reference]
  - Version: [Version if applicable]
  - Critical path: [Yes/No - is this a critical dependency?]

**Depended On By:**
- **[Dependent Project]**: [How it uses this project]
  - Integration points: [Specific interfaces/APIs used]

**Dependency Diagram:**

```plantuml
@startuml
'title: Dependency Map - [SubmoduleName]
'date: YYYY-MM-DD

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

System(current, "[SubmoduleName]")
System(dep1, "[Dependency 1]")
System(dep2, "[Dependency 2]")
System(dependent1, "[Dependent 1]")

Rel(current, dep1, "uses")
Rel(current, dep2, "uses")
Rel(dependent1, current, "uses")

@enduml
```
```

**10.3: Update Technology Stack Analysis**

Edit `docs/shared/technology-stack.md`:

Add entry for this project's technology stack and note commonalities/differences with other projects.

**Validation:**
- [ ] Cross-cutting patterns updated with this project's patterns
- [ ] Dependency map updated
- [ ] Technology stack analysis updated
- [ ] Relationships to other projects documented

### Step 11: Create Project Summary

Create concise `README.md` for quick reference.

**Template:**

```markdown
# [SubmoduleName] - Architecture Analysis

**Last Updated:** YYYY-MM-DD
**Status:** Active
**Submodule References:**
  - `<SubmoduleName>` @ `<branch>` (`<commit-hash>`)

## Overview
[One paragraph project description]

## Purpose
[What does this project do? What domain does it serve?]

## Key Technologies
- [Technology 1]
- [Technology 2]
- [Technology 3]

## Architecture
- **Type:** [Desktop App | Service | Library | Web App | Medical Device Software]
- **UI Framework:** [WPF | Web | None]
- **Data Storage:** [SQL Server | SQLite | None]
- **Pattern:** [MVVM | Layered | etc.]

## Integration Points
- [System 1]: [Purpose]
- [System 2]: [Purpose]

## Development Group
**Team:** [Team name if identifiable]
**Standards:** [Unique standards or practices observed]

## Quick Links
- [Architecture Details](./architecture.md)
- [Patterns and Practices](./patterns.md)
- [Technology Stack](./technologies.md)
- [C4 Diagrams](../../diagrams/<SubmoduleName>/)

## Key Findings
- [Important finding 1]
- [Important finding 2]
- [Important finding 3]

## Change Log
- YYYY-MM-DD v1.0: Initial analysis
```

**Validation:**
- [ ] Overview is concise (one paragraph)
- [ ] Key technologies listed
- [ ] Integration points identified
- [ ] Links to detailed documentation work

### Step 12: Cross-Reference and Validate

Ensure consistency and completeness.

**10.1: Cross-Reference Check**

- [ ] README links to all detailed docs
- [ ] Architecture.md links to diagrams
- [ ] Patterns.md references architecture.md where appropriate
- [ ] All diagrams referenced in documentation
- [ ] Submodule version consistent across all files

**10.2: Diagram Validation**

```bash
# Test PlantUML rendering (if tool available)
plantuml -testdot
plantuml .claude/architecture/diagrams/<SubmoduleName>/*.puml
```

**10.3: Metadata Validation**

- [ ] All files have **Last Updated** date
- [ ] All files have **Status** field
- [ ] All files have **Submodule References** with branch and commit
- [ ] Change logs updated

**10.4: Style Guide Compliance**

- [ ] Follows [Documentation Style Guide](./documentation-style-guide.md)
- [ ] Consistent heading hierarchy
- [ ] Code blocks have language identifiers
- [ ] Tables properly formatted
- [ ] Terminology consistent


### Step 13: Update CLAUDE.md

Add architectural insights to root CLAUDE.md.

Add to "Key Architectural Insights" section or create project-specific notes.

**Validation:**
- [ ] CLAUDE.md updated with key findings
- [ ] Links to detailed analysis added
- [ ] Cross-project implications noted

## Post-Analysis Activities

### Documentation Review

Schedule review of analysis:
- [ ] Self-review completed using style guide checklist
- [ ] Diagrams render correctly
- [ ] Code references are accurate
- [ ] Patterns make sense

### Future Analysis Tracking

Record when next review should occur:
```markdown
## Next Review
**Scheduled:** YYYY-MM-DD (3 months from now)
**Trigger Events:**
- Major architectural refactoring
- New integration points added
- Technology stack changes
```

## Validation Checklist

Complete analysis validation:

- [ ] Submodule metadata captured (branch, commit, date)
- [ ] Directory structure created
- [ ] Initial discovery completed
- [ ] Technology stack documented
- [ ] C4 Context diagram created and validated
- [ ] C4 Container diagram created and validated
- [ ] Patterns identified and documented (minimum 3)
- [ ] Architecture.md completed
- [ ] README.md created
- [ ] All cross-references validated
- [ ] Diagrams render without errors
- [ ] Metadata consistent across all files
- [ ] Style guide compliance verified
- [ ] Common patterns registry updated
- [ ] CLAUDE.md updated
- [ ] Documentation review completed

## Common Issues

| Issue | Cause | Resolution |
|-------|-------|------------|
| Diagram won't render | PlantUML syntax error | Validate syntax at plantuml.com/plantuml |
| Can't determine architecture | Complex/legacy code | Focus on entry points and public interfaces |
| Too many patterns identified | Over-classification | Focus on significant, repeated patterns |
| Submodule detached HEAD | Git submodule state | `cd <submodule> && git checkout main` |
| Missing documentation in submodule | No existing docs | Analyze code structure and naming conventions |
| Conflicting patterns | Multiple teams/eras | Document both and note evolution |

## Related Protocols

- [Documentation Style Guide](./documentation-style-guide.md)
- [Submodule Review Protocol](./submodule-review.md)
- [Pattern Documentation Protocol](./pattern-documentation.md)
- [Diagram Creation Protocol](./diagram-creation.md)

## Change Log

- 2026-01-02 v1.0: Initial architectural analysis protocol created

## Notes

**Time Estimate:**
- Simple project (< 10 projects): 2-3 hours
- Medium project (10-30 projects): 4-6 hours
- Complex project (30+ projects): 8-12 hours

**Complexity Factors:**
- Size of codebase
- Number of integration points
- Available documentation
- Code organization clarity
- Number of patterns to identify
