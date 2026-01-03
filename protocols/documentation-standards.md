# Protocol: Documentation Standards

This protocol defines the standards for organizing and maintaining documentation in the GreenOnion React/TypeScript project.

---

## Documentation Structure

All important documentation must be collected under the `./docs` folder to enable wiki generation.

```
docs/
├── README.md                    # Index and navigation guide
│
├── architecture/                # System architecture and design patterns
│   ├── ARCHITECTURE.md          # GOLD STANDARD - Main architecture doc
│   ├── architecture-overview.md # High-level system overview
│   └── anti-patterns.md         # Patterns to AVOID (learn from mistakes)
│
├── api/                         # API documentation
│   ├── API_CALLS.md             # API client patterns and usage
│   ├── SCHEMA_METADATA_GUIDE.md # OpenAPI x-* extensions
│   └── api-enhancement-summary.md
│
├── overview/                    # Application overview
│   └── APPLICATION_SUMMARY.md   # Routes, components, permissions
│
├── epics/                       # Epic/feature-based documentation
│   ├── authentication/          # Auth & user management
│   │   ├── b2c-sidebar-improvements.md
│   │   ├── role-system-guide.md
│   │   └── user-management.md
│   │
│   ├── bulk-actions/            # Bulk action feature
│   │   ├── implementation-guide.md
│   │   ├── registry-guide.md
│   │   ├── backend-requirements.md
│   │   └── test-plan.md
│   │
│   ├── code-generation/         # TypeScript generator & templates
│   │   ├── typescript-generator-templates.md
│   │   ├── template-context-reference.md
│   │   ├── entity-form-generation.md
│   │   ├── swagger-extensions.md
│   │   ├── zod-integration-guide.md
│   │   ├── client-generation.md
│   │   └── cell-renderer-registry.md
│   │
│   ├── datagrid/                # DataGrid components
│   │   ├── style-guide.md
│   │   ├── generic-datagrid-design.md
│   │   ├── filter-control.md
│   │   ├── filter-sidebar-enhancement.md
│   │   ├── table-design.md
│   │   └── grid-design-notes.md
│   │
│   ├── external-sync/           # OneWorldSync integration
│   │   └── conditional-readonly-proposal.md
│   │
│   ├── ui-migration/            # UI framework migration
│   │   ├── mui-to-primereact.md
│   │   ├── primereact-migration-plan.md
│   │   ├── prime-styles.md
│   │   ├── styles-compared.md
│   │   └── ui-wrapper-architecture.md
│   │
│   └── v2-migration/            # V2 component migration
│       └── migration-plan.md
│
├── features/                    # Cross-cutting features
│   ├── application-insights.md
│   ├── CONFIGURATION.md
│   ├── correlation-tracking.md
│   └── dual-build-modes.md
│
├── changes/                     # Change history
│   └── CHANGELOG.md
│
├── status/                      # Status and tracking
│   └── SYSTEM_STATUS.md
│
└── archive/                     # Historical documentation
    ├── sessions/                # Session memory files
    └── historical/              # Completed/outdated docs
```

---

## File Location Rules

### Must Be Under ./docs

| Type | Location | Example |
|------|----------|---------|
| Architecture docs | `docs/architecture/` | ARCHITECTURE.md, anti-patterns.md |
| API documentation | `docs/api/` | API_CALLS.md, SCHEMA_METADATA_GUIDE.md |
| Epic/feature docs | `docs/epics/{epic-name}/` | bulk-actions/implementation-guide.md |
| Cross-cutting features | `docs/features/` | application-insights.md |
| Change history | `docs/changes/` | CHANGELOG.md |
| Status tracking | `docs/status/` | SYSTEM_STATUS.md |
| Historical docs | `docs/archive/` | session memory files |

### Epic-Based Organization

Features, proposals, and implementation guides should be organized by **epic** (major feature area):

| Epic | Description | Examples |
|------|-------------|----------|
| `authentication/` | Auth, roles, user management | role-system-guide.md |
| `bulk-actions/` | Bulk action feature | implementation-guide.md, test-plan.md |
| `code-generation/` | Templates, generators | swagger-extensions.md |
| `datagrid/` | DataGrid components | style-guide.md, filter-control.md |
| `external-sync/` | OneWorldSync integration | conditional-readonly-proposal.md |
| `ui-migration/` | Framework migrations | mui-to-primereact.md |
| `v2-migration/` | V2 component migration | migration-plan.md |

### Must Remain at Root

| File | Purpose |
|------|---------|
| `README.md` | Project readme |
| `CLAUDE.md` | Claude Code instructions |
| `TODO.md` | Work tracking |
| `changes.md` | Change history |

### Must Be Under .claude/

| Type | Location |
|------|----------|
| Protocols | `.claude/protocols/` |
| Settings | `.claude/settings.local.json` |

---

## Document Standards

### Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Main docs | `UPPER_CASE.md` | `ARCHITECTURE.md` |
| Guides | `kebab-case.md` | `swagger-extensions.md` |
| Test plans | `test-plan.md` (in epic folder) | `epics/bulk-actions/test-plan.md` |
| Proposals | `*-proposal.md` | `conditional-readonly-proposal.md` |
| Index files | `README.md` | `docs/README.md` |

### Anti-Patterns to Avoid

| Wrong | Right | Reason |
|-------|-------|--------|
| Date-based names: `SESSION_2025-12-31.md` | Feature-based: `bulk-actions/test-plan.md` | Features named by what, not when |
| Scattered: `/src/feature/README.md` | Collected: `docs/epics/feature/guide.md` | Wiki generation requires central docs |
| Root-level: `/CHANGES.md` | Organized: `docs/changes/CHANGELOG.md` | Only CLAUDE.md, README.md, TODO.md at root |

### Required Sections for Feature Documentation

1. **Overview** - What the feature does
2. **Usage** - Code examples (TypeScript/React)
3. **Configuration** - Props, options, metadata
4. **Examples** - Real-world usage patterns
5. **Related** - Links to related docs

### Required Sections for Proposals

1. **Status** - Proposed/Approved/Implemented
2. **Date** - When proposed/implemented
3. **Problem Statement** - What problem it solves
4. **Proposed Solution** - How it will be solved
5. **Implementation Details** - Files to modify
6. **Questions** - Open items to resolve

---

## Code Examples in Documentation

### TypeScript/React Standards

Use proper TypeScript syntax with types:

```typescript
// Good - includes types
interface BulkActionResult {
  success: boolean;
  message: string;
  affectedCount?: number;
}

const handleBulkAction = async (ids: number[]): Promise<BulkActionResult> => {
  // implementation
};
```

### Import Examples

Always show complete imports:

```typescript
// Good - shows actual import path
import { QueryManufacturerModelComboBox } from '@/components/GreenOnion';
import type { IQueryManufacturerModel } from '@/api/GreenOnion/Models';
```

---

## Consolidation Rules

### Moving Scattered Documentation

If documentation exists outside `./docs`:

1. Identify the appropriate epic folder under `docs/epics/` or category folder
2. Move the file with a descriptive name (not date-based)
3. Update all references in code and other docs
4. Update `docs/README.md` index
5. If a pattern mistake was made, document in `docs/architecture/anti-patterns.md`

### When Documentation Becomes Outdated

1. Move to `docs/archive/historical/` if historical value
2. Move session memories to `docs/archive/sessions/`
3. Delete if no value
4. Update index files

### Creating New Epic Documentation

When starting a new feature/epic:

1. Create folder: `docs/epics/{epic-name}/`
2. Add implementation guide: `implementation-guide.md` or `{feature-name}-guide.md`
3. Add test plan if testable: `test-plan.md`
4. Add proposals if design needed: `{feature}-proposal.md`
5. Update `docs/README.md` with new epic

---

## Cross-Referencing

### Internal Links

Use relative paths from the document location:

```markdown
See [Architecture](./architecture/ARCHITECTURE.md)
See [API Patterns](../api/API_CALLS.md)
```

### Referencing from Code

When referencing docs in code comments:

```typescript
/**
 * Bulk action registry for grid actions.
 * @see docs/features/BULK_ACTION_REGISTRY.md
 */
```

---

## Maintenance

### When to Update Documentation

1. After implementing new features
2. After architectural changes
3. After design decisions
4. When API patterns change
5. When templates are modified

### Review Checklist

- [ ] All docs under `./docs/` (not scattered in src/)
- [ ] Protocols in `.claude/protocols/`
- [ ] Code examples are valid TypeScript
- [ ] Import paths are correct
- [ ] Cross-references working
- [ ] `docs/README.md` index updated

---

*Protocol version: 1.1*
*Updated: 2025-12-31*
*Applies to: React/TypeScript frontend*
