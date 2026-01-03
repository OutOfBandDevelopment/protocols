# Zod Schema Integration with GenericGrid - Enhanced Edition

This guide explains how to leverage the comprehensive Zod schema metadata to automatically configure GenericGrid components in the Green Onion application with advanced features including permissions, relationships, and intelligent defaults.

## Overview

The Green Onion application has auto-generated Zod schemas in `/src/api/GreenOnion/Schema/` that contain rich metadata about each model. This metadata can be used to automatically:
- Generate column definitions with proper labeling and tooltips
- Determine filter, sort, and search capabilities per field
- Identify primary keys and foreign key relationships
- Configure role-based permissions and field access
- Apply default sorting and search behavior
- Handle internationalization keys for labels and tooltips
- Manage field visibility and read-only states

## Key Components

### 1. Zod Schema Helper Utility
**Location**: `/src/utils/zodSchemaHelper.tsx`

This utility provides functions to extract metadata from Zod schemas and convert them to GenericGrid configurations:

```typescript
import { zodSchemaToGridColumns, getSchemaMetadata, getNumericColumns } from '@/utils/zodSchemaHelper';
```

### 2. Enhanced Data Component Example
**Location**: `/src/components/Reusable/DataComponents/ZodEnhancedManufacturerData/`

Shows how to create a data component that uses Zod schema metadata for automatic configuration.

### 3. Demo Page
**Location**: `/src/pages/Development/ZodSchemaDemo/`
**Route**: `/development/zod-schema-demo`

Interactive demo showing schema analysis and auto-configuration capabilities.

## Schema Metadata

The Zod schemas contain comprehensive metadata fields organized into several categories:

### Navigation and Relationships
- `x-navigation-key`: Primary key on this model (e.g., "True" for ID fields)
- `x-navigation-target`: Class name of related model for foreign key relationships
- `x-query-set`: The entity model this query model is associated with
- `full-name`: Original server-side class name (schema-level metadata)

### Display and Labeling  
- `x-label`: Field label value (may also be i18n lookup key)
- `x-alt-text`: Alt text/tooltip (may also be i18n lookup key)
- `x-display-name`: Custom display name override
- `x-column-width`: Preferred column width
- `x-format`: Data format (currency, percentage, date, datetime, email, phone)
- `x-hidden`: Hide field from display
- `x-readonly`: Field is read-only

### Search, Filter, and Sort Behavior
- `x-searchable`: Fields that are used with the search term
- `x-not-searchable`: Fields that do not support search term query
- `x-not-filterable`: Fields that do not support filtering
- `x-not-sortable`: Fields that do not support sorting
- `x-default-sort`: Field or fields used as default sort values for this model

### Permissions and Access Control
- `x-permissions`: List of roles that are allowed to use the related operation

### Type and Formatting
- `x-column-type`: Specific column type (text, number, date, boolean, dropdown)
- `x-required`: Field validation requirements
- `x-min`/`x-max`: Numeric constraints
- `x-pattern`: Regex pattern for validation

## Usage Examples

### Basic Auto-Configuration

```typescript
import ZQueryManufacturerModel from '@/api/GreenOnion/Schema/ZQueryManufacturerModel';
import { zodSchemaToGridColumns, getSchemaMetadata } from '@/utils/zodSchemaHelper';

// Auto-generate columns from schema
const columns = zodSchemaToGridColumns<IQueryManufacturerModel>(
  ZQueryManufacturerModel,
  {
    excludeFields: ['createdById', 'updatedById'],
    customRenderers: {
      isActive: (value) => <Tag severity={value ? 'success' : 'danger'} value={value ? 'Active' : 'Inactive'} />
    }
  }
);

// Get metadata for configuration
const metadata = getSchemaMetadata(ZQueryManufacturerModel);
const primaryKey = metadata.primaryKey; // 'manufacturerId'
const numericColumns = getNumericColumns(ZQueryManufacturerModel);
```

### Using with SimpleGenericGrid

```typescript
<SimpleGenericGrid<TFilter, TOrderBy, TSearchQuery, TModel>
  client={clientInstance}
  dataKey={metadata.primaryKey}
  columns={columns}
  numericColumns={numericColumns}
  enableSearch={true}
  enableFilter={true}
  enableSort={true}
/>
```

### Custom Column Overrides

```typescript
const columns = zodSchemaToGridColumns<IQueryManufacturerModel>(
  ZQueryManufacturerModel,
  {
    columnOverrides: {
      manufacturerName: {
        width: '200px',
        header: 'Company Name'
      },
      glnNumber: {
        width: '150px',
        header: 'GLN #'
      }
    }
  }
);
```

## Available Schemas

The following Zod schemas are available for use:

### Query Models (for display)
- `ZQueryManufacturerModel`
- `ZQueryAllergenModel`
- `ZQuerySchoolDistrictModel`
- `ZQueryUserModel`
- `ZQueryProductModel`
- `ZQueryIngredientModel`
- `ZQueryErrorLogModel`
- `ZQueryUserActionLogModel`
- `ZQueryDefinedFilterModel`
- And many more...

### Save Models (for forms)
- `ZSaveManufacturerModel`
- `ZSaveAllergenModel`
- `ZSaveSchoolDistrictModel`
- `ZSaveUserModel`
- `ZSaveProductModel`
- And more...

## Benefits

### 1. Automatic Configuration
- Column definitions generated from schema
- Filter types determined by data types
- Sortable/filterable flags from metadata
- Primary key identification

### 2. Type Safety
- Full TypeScript integration
- Compile-time type checking
- IntelliSense support
- Runtime validation available

### 3. Maintainability
- Single source of truth for data models
- Schema changes automatically propagate
- Reduced boilerplate code
- Consistent behavior across components

### 4. Rich Metadata
- Searchability flags
- Navigation relationships
- Display formatting hints
- Validation rules

## Implementation Strategy

1. **Start with auto-configuration**: Use `zodSchemaToGridColumns()` for initial setup
2. **Add custom renderers**: Define custom display logic for specific fields
3. **Override as needed**: Use `columnOverrides` for fine-tuning
4. **Leverage metadata**: Use schema metadata for advanced features

## Future Enhancements

The Zod schema integration can be extended to support:
- Form generation with validation
- Dynamic filter building
- Relationship navigation
- Export/import configurations
- API request/response validation

## Troubleshooting

### Schema Not Found
If a schema is not found, check:
1. The schema exists in `/src/api/GreenOnion/Schema/`
2. The interface name mapping in `createSchemaLookup()` is correct
3. The import path is correct

### Missing Metadata
If metadata is missing:
1. Check the schema file for the metadata definitions
2. Ensure the metadata extraction is working correctly
3. Consider adding custom metadata through schema extensions

### Type Errors
If you encounter type errors:
1. Ensure the generic types match between schema and usage
2. Check that the interface types are properly imported
3. Verify the schema structure matches the expected model

## Summary

The Zod schema integration provides a powerful way to automatically configure GenericGrid components while maintaining type safety and reducing boilerplate code. By leveraging the rich metadata in the schemas, you can create sophisticated data grids with minimal manual configuration.