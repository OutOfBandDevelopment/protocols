# Template Development Protocol

This protocol defines the workflow for developing, fixing, and enhancing Handlebars templates used for code generation in this project.

## Core Principle

**NEVER modify generated code directly. Always modify the template that generates the code.**

Generated files are overwritten during the build process. Any direct modifications will be lost.

## Template Locations

| Template Type | Location |
|---------------|----------|
| Zod Schemas | `TypeScriptGenerator/api/GreenOnion/Schema/{Entity}Schema.ts.hbs` |
| Components | `TypeScriptGenerator/components/GreenOnion/_Entity_/` |
| Pages | `TypeScriptGenerator/pages/GreenOnion/_Entity_/` |
| Routes | `TypeScriptGenerator/pages/GreenOnion/Routes.tsx.hbs` |
| Common Helpers | `TypeScriptGenerator/_common/` |

## Workflow Steps

### 1. Understand the Request

Before making any changes:

1. **Clarify requirements** with the requester
   - What functionality is needed?
   - What is the expected output?
   - Are there existing patterns to follow?

2. **Identify affected templates**
   - Which template(s) generate the code in question?
   - What data sources feed into the template (swagger.json, metadata)?

3. **Review existing generated output**
   - Read the currently generated file to understand the baseline
   - Identify what needs to change

### 2. Analyze the Data Source

Templates consume data from `swagger.json`. Before modifying templates:

1. **Check swagger.json structure**
   ```bash
   # View model definitions
   grep -A 50 '"ModelName":' swagger.json
   ```

2. **Understand available properties**
   - Standard OpenAPI: `type`, `properties`, `required`, `minLength`, `maxLength`, `pattern`, `format`
   - Custom extensions: `x-label`, `x-bulk-actions`, `x-navigation-target`, etc.

3. **Document new metadata** if adding custom extensions
   - Update `SCHEMA_METADATA_GUIDE.md`

### 3. Modify the Template

#### Handlebars Syntax Guidelines

```handlebars
{{!-- Safe comment syntax (no ~ whitespace control in comments) --}}

{{!-- String property --}}
{{#if property}}
    'property': '{{property}}',
{{/if}}

{{!-- Array property --}}
{{#if arrayProperty}}
    'arrayProperty': [{{#each arrayProperty as |item|}}'{{item}}',{{/each}}],
{{/if}}

{{!-- Conditional logic --}}
{{#if condition}}
    {{!-- true branch --}}
{{else}}
    {{!-- false branch --}}
{{/if}}

{{!-- Iteration --}}
{{#each items as |item|}}
    {{item.name}}: {{item.value}}
{{/each}}
```

#### Common Pitfalls

1. **Comment syntax**: Avoid `{{~!-- --~}}` - can cause parsing errors
2. **Special characters**: Be careful with properties containing quotes, newlines, or special chars
3. **Array vs string detection**: Handlebars doesn't easily distinguish types - use explicit property handling
4. **Whitespace control**: Use `{{~` and `~}}` carefully for output formatting

### 4. Test the Changes

1. **Generate the output**
   ```bash
   ./get-swagger.bat   # or equivalent generation command
   ```

2. **Review generated files**
   - Compare with expected output
   - Check for syntax errors
   - Verify all edge cases

3. **Build verification**
   ```bash
   npm run build
   ```
   - Must complete without TypeScript errors
   - Check for runtime warnings

4. **Manual testing**
   - Test the feature in the running application
   - Verify expected behavior

### 5. Update Documentation

After successful implementation:

1. **Update relevant docs**
   - `SCHEMA_METADATA_GUIDE.md` for new metadata extensions
   - Feature-specific docs (e.g., `BULK_ACTION_REGISTRY.md`)
   - `ARCHITECTURE.md` if architectural patterns changed

2. **Add inline comments** to templates for complex logic

3. **Update `changes.md`** with:
   - Date and description
   - Files modified
   - Before/after examples

## Collaboration Pattern

### When Working with Requesters

1. **Initial discussion**
   - Understand the full scope of the request
   - Identify all affected templates and generated files
   - Agree on expected output format

2. **Iterative development**
   - Make incremental changes
   - Share generated output for review
   - Incorporate feedback before finalizing

3. **Verification**
   - Confirm generated output matches expectations
   - Validate build passes
   - Test functionality in application

### Communication Checkpoints

| Stage | Action |
|-------|--------|
| Before starting | Confirm understanding of requirements |
| After template changes | Show generated output sample |
| After testing | Report build status and test results |
| After completion | Summarize changes and updated documentation |

## Example: Adding New Schema Metadata

### Scenario
Add support for `x-custom-property` array in Zod schema metadata.

### Steps

1. **Update swagger.json** (or verify property exists)
   ```json
   {
     "QueryExampleModel": {
       "x-custom-property": ["value1", "value2"]
     }
   }
   ```

2. **Modify template** (`{Entity}Schema.ts.hbs`)
   ```handlebars
   {{#if definition.x-custom-property}}
       'x-custom-property': [{{#each definition.x-custom-property as |val|}}'{{val}}',{{/each}}],
   {{/if}}
   ```

3. **Generate and verify**
   ```bash
   ./get-swagger.bat
   # Check generated file shows:
   # 'x-custom-property': ['value1','value2',],
   ```

4. **Build and test**
   ```bash
   npm run build
   ```

5. **Document in `SCHEMA_METADATA_GUIDE.md`**
   ```markdown
   ### x-custom-property
   - **Type**: `string[]`
   - **Purpose**: Description of what this property does
   - **Example**: `["value1", "value2"]`
   ```

## Troubleshooting

### Template Parsing Errors

**Error**: "Reached end of template in the middle of a comment"
- **Cause**: Comments with `~` whitespace control
- **Fix**: Use simple `{{!-- comment --}}` syntax

**Error**: Unexpected output format
- **Cause**: Handlebars treating arrays as strings
- **Fix**: Use explicit `{{#each}}` for array properties

### Build Errors After Generation

**Error**: TypeScript compilation fails
- **Check**: Generated syntax is valid TypeScript
- **Check**: All imports are correct
- **Check**: Property names match expected interfaces

**Error**: Runtime errors
- **Check**: Generated code logic is correct
- **Check**: Null/undefined handling

## Registry Pattern Integration

When templates generate code that uses registries:

1. **Cell Renderer Registry** (`/src/utils/cellRendererRegistry.tsx`)
   - Template uses `x-custom-renderer` metadata
   - Generates appropriate `body` functions

2. **Bulk Action Registry** (`/src/utils/bulkActionRegistry.tsx`)
   - Template uses `x-bulk-actions` metadata
   - Generates `bulkActions` prop array

3. **Component Registry** (`componentRegistry.ts.hbs`)
   - Generates lazy-loaded component mappings
   - Uses `x-navigation-target` for lookups

## Checklist

Before marking template work complete:

- [ ] Template changes are in the correct `.hbs` file(s)
- [ ] Generated output matches expected format
- [ ] Build passes without errors (`npm run build`)
- [ ] Feature works in running application
- [ ] Documentation updated (SCHEMA_METADATA_GUIDE.md, feature docs)
- [ ] Changes logged in `changes.md`
- [ ] No direct modifications to generated files
