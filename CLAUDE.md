# Claude Code Configuration

**Last Updated:** 2026-01-16

---

## Project Overview

This repository contains reusable protocols for software development workflows.

---

## Protocols Location

All protocols are in `./protocols/` organized by category:

| Category        | Path                     | Purpose                            |
|-----------------|--------------------------|----------------------------------- |
| Documentation   | `protocols/documentation/`| Documentation standards and guides |
| Research        | `protocols/research/`    | Technical research and references  |
| Projects        | `protocols/projects/`    | Project management and reviews     |
| Maintenance     | `protocols/maintenance/` | Repository maintenance workflows   |
| Software        | `protocols/software/`    | Analysis and validation            |
| Workflows       | `protocols/workflows/`   | Development lifecycle              |

See [protocols/README.md](./protocols/README.md) for the complete registry (36 protocols across 6 categories).

---

## Naming Conventions

### Files

| Type                | Convention      | Example                         |
|---------------------|-----------------|-------------------------------- |
| Protocol files      | kebab-case      | `documentation-style-guide.md`  |
| Documentation files | kebab-case      | `feature-implementation.md`     |
| Category folders    | lowercase       | `documentation/`, `workflows/`  |

### Do NOT Use

- SCREAMING-KEBAB-CASE (e.g., `MY-PROTOCOL.md`)
- snake_case (e.g., `my_protocol.md`)
- Spaces in filenames
- Date-based naming (e.g., `SESSION_2025-12-31.md`)
- Generic names (e.g., `CHANGES.md`, `NOTES.md`)

---

## Table Formatting Rules

When creating markdown tables, align pipe characters for readability:

```markdown
Good:
| Column 1     | Column 2        | Column 3     |
|--------------|-----------------|--------------|
| Short        | Medium value    | Longer value |
| Another      | Value           | Here         |

Bad:
| Column 1 | Column 2 | Column 3 |
|---|---|---|
| Short | Medium value | Longer value |
```

---

## Protocol Standards

### Required Sections

Every protocol must include:

1. Title and metadata (version, date, purpose)
2. Overview
3. When to Use
4. Prerequisites
5. Procedure (step-by-step)
6. Validation Checklist
7. Common Issues
8. Related Protocols
9. Change Log

### Metadata Format

```markdown
# Protocol Title

**Version:** X.Y
**Last Updated:** YYYY-MM-DD
**Purpose:** One sentence describing the protocol's purpose
```

---

## Content Rules

### Prohibited Content

- Company names, project names, or other identifiable information
- Time estimates or effort estimates
- Complexity assessments without measurable criteria
- Business value propositions or ROI calculations
- Hypothetical personas or workflows
- Marketing materials

### Required Content

- Generic placeholders: `{ProjectName}`, `{FeatureName}`, `{ComponentName}`
- Code references with file paths and line numbers
- Actual implementation details from code analysis
- Measurable criteria for any assessments

---

## Documentation Standards

### Root Files Only

Only these files at project root:
- `README.md` - Project readme
- `CLAUDE.md` - This file
- `TODO.md` - Work tracking

### All Other Documentation

Place under `./docs/` following the structure in `documentation-standards.md`.

---

## Development Workflow

Use the workflow protocols in this order:

```
1. init                      <- Project initialization (first time)
   |
2. start work on {id}        <- Create branch and docs
   |
3. [Implement changes]       <- Write code
   |
4. review my changes         <- Code review
   |
5. create test plan          <- Test planning
   |
6. [Implement & run tests]   <- Execute tests
   |
7. [Create pull request]     <- Submit for review
```

---

## Trigger Phrases

| Phrase                           | Protocol                               |
|----------------------------------|----------------------------------------|
| `init`                           | `workflows/init-setup.md`              |
| `setup project`                  | `workflows/init-setup.md`              |
| `start work on {number}`         | `workflows/start-work.md`              |
| `review my changes`              | `workflows/change-review.md`           |
| `create test plan`               | `workflows/create-test-plan.md`        |
| `check incoming for updates`     | `workflows/protocol-import.md`         |
| `I have new protocols`           | `workflows/protocol-import.md`         |
| `import protocols`               | `workflows/protocol-import.md`         |
| `process incoming protocols`     | `workflows/protocol-import.md`         |
| `review incoming`                | `workflows/protocol-import.md`         |
| `show lifecycle`                 | `workflows/lifecycle.md`               |
| `show workflow`                  | `workflows/lifecycle.md`               |

---

## User Preferences (Collected)

The following preferences were collected and should be maintained:

| Preference                  | Value                                  |
|-----------------------------|----------------------------------------|
| Naming pattern              | kebab-case                             |
| Protocol organization       | Subfolders by category                 |
| Project consolidation       | Generic/unnamed (no project-specific)  |
| Documentation style guide   | v2.1 (technical focus)                 |
| Task management version     | v1.1 (with features folder)            |
| Documentation standards     | Merged best of both versions           |
| Table formatting            | Aligned pipe characters                |
| Identifiable names          | Must be sanitized/genericized          |

---

## Change Log

- 2026-01-16: Updated protocols location table; added research, projects, and maintenance categories; registry now contains 36 protocols
- 2026-01-03: Initial CLAUDE.md created with consolidated rules
