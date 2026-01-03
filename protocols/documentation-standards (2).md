# Protocol: Documentation Standards

This protocol defines the standards for organizing and maintaining documentation in the GreenOnion project.

---

## Root Directory Rules

**ONLY these files are allowed at project root:**

| File | Purpose |
|------|---------|
| `README.md` | Project readme |
| `CLAUDE.md` | Claude Code memory |
| `TODO.md` | Work tracking |

**All other documentation MUST be under `./docs/`**

---

## Documentation Structure

All documentation must be collected under the `./docs` folder to enable wiki generation.

```
docs/
├── architecture/           # Architecture documents
│   ├── README.md          # TOC and entry point
│   ├── system-overview.md
│   ├── solution-structure.md
│   ├── layer-architecture.md
│   ├── api-patterns.md
│   ├── security-model.md
│   ├── design-decisions.md
│   └── bulk-actions-design.md
│
├── api/                    # API reference documentation
│   ├── endpoints.md
│   ├── data-models.md
│   └── query-pattern.md
│
├── changes/                # Change history
│   └── CHANGELOG.md       # Version history
│
├── data-sync/              # External data sync
│   ├── OneWorldSync-to-Products.md
│   └── SUGGESTED_SYNC_UPDATE.md
│
├── proposals/              # Design proposals (epic/feature naming)
│   ├── bulk-actions.md
│   └── external-sync-readonly.md
│
├── templates/              # Code generation templates
│   └── TypeScriptGenerator/
│
├── swagger-extensions.md   # OpenAPI extensions reference
├── bulk-actions-guide.md   # Frontend implementation guide
└── HandleBarsExtended.md   # Handlebars helpers reference
```

---

## File Location Rules

| Type | Location | Example |
|------|----------|---------|
| Architecture docs | `docs/architecture/` | `layer-architecture.md` |
| API documentation | `docs/api/` | `endpoints.md` |
| Change history | `docs/changes/` | `CHANGELOG.md` |
| Data sync docs | `docs/data-sync/` | `OneWorldSync-to-Products.md` |
| Design proposals | `docs/proposals/` | `bulk-actions.md` |
| Extension references | `docs/` | `swagger-extensions.md` |
| Implementation guides | `docs/` | `bulk-actions-guide.md` |
| Protocols | `.claude/protocols/` | `documentation-standards.md` |

---

## Naming Conventions

### Epic/Feature Naming for Proposals

Proposals, suggested changes, and feature documents MUST use **epic/feature style naming**:

| Style | Convention | Example |
|-------|------------|---------|
| Epic | `{epic-name}.md` | `bulk-actions.md` |
| Feature | `{feature-name}.md` | `external-sync-readonly.md` |
| Sub-feature | `{epic}-{feature}.md` | `bulk-actions-allocate.md` |

**DO NOT use:**
- Generic names like `SUGGESTED_CHANGES.md` or `CHANGES.md`
- All-caps naming (except README.md, CHANGELOG.md)
- Underscores (use kebab-case)

### General Naming

| Type | Convention | Example |
|------|------------|---------|
| Architecture | `kebab-case.md` | `layer-architecture.md` |
| Proposals | `kebab-case.md` | `bulk-actions.md` |
| Index files | `README.md` | `docs/architecture/README.md` |
| Changelogs | `CHANGELOG.md` | `docs/changes/CHANGELOG.md` |

---

## Document Standards

### Required Sections

#### Architecture Documents

1. Document Information (date, status)
2. Overview/Purpose
3. Diagrams (PlantUML)
4. Details/Implementation
5. Related Documents

#### Proposals

1. Status (Proposed/Implemented)
2. Date Proposed/Implemented
3. Summary
4. Problem Statement
5. Solution
6. Implementation Files
7. Related Documentation

### PlantUML Guidelines

- Use `!theme plain` for consistent styling
- Use `skinparam linetype ortho` for clean lines
- Test diagrams before committing
- Keep diagrams focused (one concept per diagram)

---

## Consolidation Rules

### Moving Root-Level Files

If documentation exists at project root (other than README.md, CLAUDE.md, TODO.md):

1. Identify appropriate `docs/` subdirectory
2. Rename to epic/feature style if needed
3. Move to `docs/` subdirectory
4. Update any references in CLAUDE.md
5. Delete original file

### Moving Source-Level Docs

If documentation exists in source directories (e.g., `src/GreenOnion.Common/notes.md`):

1. Move to appropriate `docs/` subdirectory
2. Update any references
3. Delete original file

### Change History Files

All changelog and version history files go in `docs/changes/`:
- `CHANGELOG.md` - Main version history
- `{version}.md` - Version-specific release notes (if needed)

---

## Cross-Referencing

### Internal Links

Use relative paths:
```markdown
See [Layer Architecture](./layer-architecture.md)
See [OpenAPI Extensions](../swagger-extensions.md)
```

### Document Index

Every `docs/` subdirectory should have a `README.md` with:
- Quick navigation table
- Document index
- Related documentation links

---

## Maintenance

### When to Update Documentation

1. After implementing new features
2. After architectural changes
3. After design decisions
4. When patterns change

### Review Checklist

- [ ] Only README.md, CLAUDE.md, TODO.md at root
- [ ] All docs under `./docs/` (not scattered in source)
- [ ] Protocols in `.claude/protocols/`
- [ ] Proposals use epic/feature naming
- [ ] PlantUML diagrams validated
- [ ] Cross-references working
- [ ] Status fields updated

---

*Protocol version: 1.1*
*Created: 2025-12-31*
*Updated: 2025-12-31 - Added root file restrictions, epic/feature naming, docs/changes/*
