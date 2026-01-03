# Protocol: Template and Swagger Extension Documentation Maintenance

This protocol defines procedures for reviewing and maintaining documentation related to the TypeScript code generation templates, Swagger/OpenAPI extensions, and template engine features.

## Overview

The GreenOnion project uses a code generation pipeline that transforms OpenAPI/Swagger metadata into TypeScript clients, components, and pages. This protocol ensures documentation stays synchronized with implementation changes.

## Key Locations

### Source Files

| Component | Location | Description |
|-----------|----------|-------------|
| **Template Source** | `docs/templates/TypeScriptGenerator/` | Handlebars templates (62 files) |
| **Template Output** | `publish/TypeScriptClient/` | Generated TypeScript files |
| **Swagger Data** | `docs/swagger.json` | OpenAPI spec with extensions |
| **Entity Models** | `src/GreenOnion.Common/Models/` | C# models with metadata attributes |
| **Metadata Attributes** | `src/Common/Eliassen.Common.Abstractions/System/ComponentModel/Metadata/` | Attribute definitions |
| **Schema Filters** | `src/Common/Eliassen.Common.AspNetCore/Mvc/SwaggerGen/` | Swagger extension processors |
| **Handlebars Helpers** | `src/Common/Eliassen.Common.Extensions/Handlebars/` | Custom template helpers |
| **Template Engine CLI** | `src/Common/Eliassen.TemplateEngine.Cli/` | Code generation tool |

### Documentation Files

| Document | Location | Purpose |
|----------|----------|---------|
| **CLAUDE.md** | `/CLAUDE.md` | Project memory and guidelines |
| **Swagger Extensions** | `docs/swagger-extensions.md` | OpenAPI extension reference |
| **Handlebars Helpers** | `docs/HandleBarsExtended.md` | Template helper documentation |
| **Template README** | `docs/templates/TypeScriptGenerator/README.md` | Template usage guide |

## Review Procedures

### 1. Template Review Protocol

When reviewing or updating templates:

```bash
# Step 1: Verify current template structure
find docs/templates/TypeScriptGenerator -name "*.hbs" | wc -l

# Step 2: Run template generation to verify output
cd docs/templates/TypeScriptGenerator
call builder.bat

# Step 3: Compare generated output
diff -r publish/TypeScriptClient/ <previous_output>
```

**Key Template Files to Review:**
- `_common/file_header.ts.hbs` - Header template for all files
- `_common/schema-template.hbs` - TypeScript type mapping
- `_common/entity-definition.hbs` - Entity metadata table
- `_common/get-entity-label.hbs` - Label extraction
- `components/GreenOnion/_Entity_/ComboBox.tsx.hbs` - Dropdown component
- `components/GreenOnion/_Entity_/MultiSelect.tsx.hbs` - Multi-select component
- `components/GreenOnion/_Entity_/DataGrid.tsx.hbs` - Data grid component
- `components/GreenOnion/_Entity_/PropertyEditor.tsx.hbs` - Form editor
- `components/GreenOnion/_Entity_/FormSchema.ts.hbs` - Zod schema generation

**Template Variable Inheritance:**
- `call` helper does NOT automatically inherit parent context
- Must explicitly pass `def=@def` to child templates for schema access
- `@root` provides access to full swagger document

### 2. Swagger Extension Review Protocol

When reviewing or updating swagger extensions:

```bash
# Step 1: Count metadata attributes
find src/Common/Eliassen.Common.Abstractions/System/ComponentModel/Metadata -name "*.cs" | wc -l

# Step 2: Extract extension names from swagger.json
cat docs/swagger.json | grep -o '"x-[^"]*"' | sort | uniq

# Step 3: Verify MetadataSchemaFilter processes all attributes
cat src/Common/Eliassen.Common.AspNetCore/Mvc/SwaggerGen/MetadataSchemaFilter.cs
```

**Extension Categories:**
1. **Display & UI (11):** x-label, x-alt-text, x-field-set, x-hidden-column, x-hidden-field, x-column-priority, x-column-tooltip, x-style-class-name, x-display-format, x-display-value, x-cell-renderer
2. **Navigation (5):** x-navigation-key, x-navigation-target, x-navigation-description, x-navigation-relation, x-navigation-variant
3. **ComboBox Variants (1):** x-combobox-variants (from ComboBoxVariant* attributes)
4. **Form Fields (5):** x-placeholder, x-help-text, x-default-value, x-validation-pattern, x-validation-message
5. **Data Operations (4):** x-quick-filter, x-export-exclude, x-bulk-edit-disabled, x-bulk-actions
6. **Entity Config (4):** x-query-model, x-save-model, x-not-creatable, x-not-selectable
7. **Search (8):** x-default-sort, x-filterable, x-searchable, x-not-filterable, x-not-sortable, x-not-searchable, x-search-term-default, x-ignore-string-comparison-replacement
8. **Operation (3):** x-query-set, x-query-action, x-permissions
9. **Document (2):** x-global-headers, x-search-capabilities

### 3. Entity Model Review Protocol

When reviewing entity models:

```bash
# Step 1: List all Query models
find src/GreenOnion.Common/Models -name "Query*.cs"

# Step 2: Check model attributes
grep -l "ComboBoxVariant\|NavigationVariant" src/GreenOnion.Common/Models/*.cs

# Step 3: Verify swagger output for specific model
cat docs/swagger.json | python3 -c "
import json,sys
d=json.load(sys.stdin)
schema = d['components']['schemas']['GreenOnion.Common.Models.QuerySchoolDistrictModel']
print(json.dumps(schema, indent=2))
"
```

**Model Attribute Checklist:**
- [ ] `[Label("...")]` - Entity display name
- [ ] `[SaveModel(typeof(...))]` - Save model mapping
- [ ] `[ComboBoxVariant(...)]` - Dropdown variants (if applicable)
- [ ] `[NavigationKey]` - Primary key property
- [ ] `[DisplayValue]` - Display text property
- [ ] `[NavigationTarget(...)]` - Foreign key relationships
- [ ] `[NavigationVariant(...)]` - Variant selection for navigation

### 4. Template Engine Review Protocol

When reviewing template engine:

```bash
# Step 1: Check CLI options
cat src/Common/Eliassen.TemplateEngine.Cli/TemplateEngineOptions.cs

# Step 2: Review available Handlebars helpers
find src/Common/Eliassen.Common.Extensions/Handlebars -name "*Descriptor.cs"

# Step 3: Run template with verbose output
cd docs/templates/TypeScriptGenerator
SET DotnetCommand=dotnet run --project "src\Common\Eliassen.TemplateEngine.Cli"
%DotnetCommand% --input docs/swagger.json --output test.md --Template GeneratedClient.md
```

## Documentation Update Checklist

When updating documentation, ensure:

### CLAUDE.md Updates
- [ ] Update metadata attribute count if changed
- [ ] Document new OpenAPI extensions
- [ ] Add new template files to structure
- [ ] Record implementation dates
- [ ] Update version history

### swagger-extensions.md Updates
- [ ] Add new attribute documentation sections
- [ ] Update Quick Reference Table
- [ ] Add usage examples for new extensions
- [ ] Update Integration Notes
- [ ] Add version history entry

### Template README Updates
- [ ] Document new template files
- [ ] Update OpenAPI Extension Support table
- [ ] Add troubleshooting entries
- [ ] Update version history

### HandleBarsExtended.md Updates
- [ ] Document new helpers
- [ ] Add usage examples
- [ ] Update architecture section if needed

## Validation Commands

### Verify Template Generation

```bash
cd docs/templates/TypeScriptGenerator

# Clean and regenerate
rmdir /s/q ..\..\..\.publish\TypeScriptClient\api\GreenOnion
rmdir /s/q ..\..\..\.publish\TypeScriptClient\components\GreenOnion
rmdir /s/q ..\..\..\.publish\TypeScriptClient\pages\GreenOnion

# Run builder
call builder.bat

# Verify output
dir /s publish\TypeScriptClient\*.tsx | find /c ".tsx"
```

### Verify Swagger Extensions

```bash
# Regenerate swagger.json
dotnet build src/GreenOnion.API
dotnet run --project src/GreenOnion.API -- --urls=http://localhost:5000
curl http://localhost:5000/swagger/all/swagger.json > docs/swagger.json

# Verify extension count
cat docs/swagger.json | grep -o '"x-[^"]*"' | sort | uniq -c | sort -rn
```

### Verify Attribute Implementation

```csharp
// Use this query to find all IMetadata implementations
var metadataTypes = typeof(IMetadata).Assembly
    .GetTypes()
    .Where(t => typeof(IMetadata).IsAssignableFrom(t) && t.IsClass && !t.IsAbstract);
```

## Common Issues

### Issue: Template variable not available

**Symptom:** `@def.x-some-extension` returns empty/undefined

**Cause:** Parent template didn't pass `def=@def` in `call` helper

**Fix:** Add `def=@def` parameter to `call` invocation:
```handlebars
{{~call template="components/GreenOnion/_Entity_/ComboBox.tsx"
        name=(concat "components/GreenOnion/" @entityNameSimple "/ComboBox")
        def=@def
        }}
```

### Issue: New attribute not appearing in swagger

**Symptom:** Attribute on model not generating x-extension

**Cause:** Attribute doesn't implement `IMetadata` or isn't processed by filter

**Fix:**
1. Ensure attribute implements `IMetadata` interface
2. Verify `MetadataSchemaFilter` processes the attribute type
3. Check attribute target (class vs property level)

### Issue: Generated TypeScript has type errors

**Symptom:** Generated .tsx files have TypeScript compilation errors

**Cause:** Template schema mapping issue

**Fix:** Review `_common/schema-template.hbs` for type mapping logic

## Sync Procedures

### Sync Templates to UI Project

```bash
cd docs/templates/TypeScriptGenerator
call sync-templates.bat
```

### Sync Generated Output

```bash
cd docs/templates/TypeScriptGenerator
call zod-demo.bat
```

## Related Protocols

- Template development workflow (GreenOnion.UI repository)
- Swagger generation and API documentation
- Entity model design patterns

## Version History

- **2025-12-30**: Initial protocol creation
- Documents 33 metadata attributes, 62 templates, template engine CLI

## Maintenance

This protocol should be reviewed and updated when:
- New metadata attributes are added
- Template structure changes
- Template engine features are added
- Documentation files are reorganized
