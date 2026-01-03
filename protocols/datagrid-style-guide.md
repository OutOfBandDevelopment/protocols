# SimpleGenericGrid Style Guide (Updated 2025-08-10)

This guide establishes the current standard for SimpleGenericGrid component usage across the Green Onion application, reflecting the successful migration from DataGrid to modern PrimeReact-based table components.

## Migration Status Notice

**Current Implementation**: All examples use SimpleGenericGrid with PrimeReact DataTable and modern client patterns. The original DataGrid component using TanStack React Table is deprecated as of 2025-08-10.

**API Client Support**: Examples support both current production (`@/api/GreenOnionClient`) and future modular (`@/api/GreenOnion/Clients/*`) patterns.

## Table of Contents
- [Overview](#overview)
- [Gold Standard Implementation](#gold-standard-implementation)
- [Required Props Configuration](#required-props-configuration)
- [Column Definition Standards](#column-definition-standards)
- [Component Structure](#component-structure)
- [Styling Standards](#styling-standards)
- [TypeScript Patterns](#typescript-patterns)
- [Best Practices](#best-practices)
- [Examples](#examples)
- [Migration Checklist](#migration-checklist)

## Overview

SimpleGenericGrid is the current standardized solution for all tabular data display in the Green Onion application. Built on PrimeReact DataTable, it provides modern UI components, advanced features, and consistent user experience.

### Design Principles
1. **Modern UI**: PrimeReact components with consistent styling and behavior
2. **Type Safety**: 4-generic pattern with full TypeScript integration
3. **Feature Completeness**: Advanced filtering, bulk selection, column management
4. **Performance**: Server-side operations with optimized rendering
5. **Accessibility**: PrimeReact's built-in accessibility standards

### Key Advantages Over Legacy DataGrid
- **Better Performance**: PrimeReact DataTable with virtual scrolling
- **Enhanced UX**: Filter sidebar, bulk operations, column reordering
- **Modern Styling**: Consistent PrimeReact theme integration
- **Type Safety**: 4-generic pattern reduces type errors
- **Maintainability**: Simplified props and configuration

## Gold Standard Implementation

Based on `/src/pages/Setting/ManageManuFacturer/index.tsx`:

```typescript
import React, { useMemo } from "react";
import { createColumnHelper } from "@tanstack/react-table";
import { Button } from "@mui/material";
import { useNavigate } from "react-router-dom";
import { 
  ManufacturerClient, 
  QueryManufacturerModel,
  QueryManufacturerModel3,
} from "@/api/GreenOnionClient"; // Current production client
import edit_icon from "@/assets/icons/edit_icon.svg";
import DataGrid from "@/components/Reusable/DataGrid";
import "./index.scss";

const columnHelper = createColumnHelper<QueryManufacturerModel3>();

export default function ManageManuFacturer() {
  const navigate = useNavigate();

  const handleCreate = () => {
    navigate(`/settings/manufacturers/edit`);
  };

  const handleEdit = (id: number) => {
    navigate(`/settings/manufacturers/edit/${id}`);
  };

  // Define columns using columnHelper for type safety
  const columns = useMemo(
    () => [
      // Column definitions...
    ],
    [handleEdit]
  );

  // Define numeric columns for proper filtering
  const numericColumns = ['manufacturerId', 'userCount', 'activeUserCount', 'productCount', 'oneWorldSyncCount'];

  return (
    <section className="manage-manufacturer-page">
      <div className="main_wrapper">
        <div className="dis_main_box min_heigh_70vh br-5">
          <div className="title f-15-700">
            <h5 className="title_text m-0">Manufacturer List</h5>
          </div>

          <DataGrid<QueryManufacturerModel3, ManufacturerClient, QueryManufacturerModel>
            title="Manufacturer List"
            createButtonText="Create Manufacturer"
            onCreateClick={handleCreate}
            clientClass={ManufacturerClient}
            queryModelClass={QueryManufacturerModel}
            columns={columns}
            numericColumns={numericColumns}
            enableSearch={true}
            enableFilter={true}
            enableSort={true}
            enableColumnVisibility={true}
            initialPageSize={20}
            className="manufacturer-grid"
          />
        </div>
      </div>
    </section>
  );
}
```

## Required Props Configuration

### Mandatory Props

```typescript
<DataGrid<TData, TClient, TQueryModel>
  title="[Page Title]"                    // Display title
  clientClass={ClientClass}               // API client constructor
  queryModelClass={QueryModelClass}       // Query model constructor  
  columns={columns}                       // Column definitions
  numericColumns={numericColumns}         // Array of numeric column IDs
  enableSearch={true}                     // Global search functionality
  enableFilter={true}                     // Column filtering with modals
  enableSort={true}                       // Multi-column sorting
  enableColumnVisibility={true}           // Column show/hide controls
  initialPageSize={20}                    // Default page size
  className="[page-name]-grid"            // Component-specific CSS class
/>
```

### Optional Props

```typescript
// When create functionality is available
createButtonText="Create [Entity]"       // Button text
onCreateClick={handleCreate}             // Create handler

// Advanced configurations
searchPlaceholder="Search [entities]..."  // Custom search placeholder
lockedFilters={lockedFilters}            // Pre-applied non-removable filters
initialFilter={initialFilter}           // Default filter state
onDataFetch={onDataFetch}               // Data fetch callback
```

## Column Definition Standards

### Basic Column Pattern

```typescript
const columnHelper = createColumnHelper<TData>();

const columns = useMemo(
  () => [
    // ID Column (first column)
    columnHelper.accessor("entityId", {
      id: "entityId",
      header: "ID",
      cell: (info) => info.getValue(),
    }),
    
    // Actions Column (second column)
    columnHelper.display({
      id: "actions",
      header: "Actions",
      enableSorting: false,
      enableColumnFilter: false,
      cell: (info) => (
        <span className="control_btn d-flex align-items-center">
          <Button
            className="actin_btn"
            onClick={() => handleEdit(info.row.original.entityId!)}
          >
            <img className="edit" src={edit_icon} alt="" />
          </Button>
        </span>
      ),
    }),
    
    // Data columns...
  ],
  [handleEdit] // Dependencies for useMemo
);
```

### Column Ordering Standards

#### Column Order (Both Management Patterns)
1. **ID Column** - First column (entityId, manufacturerId, userId, etc.)
2. **Actions Column** - Second column (edit button only for both patterns)
3. **Primary Name Column** - Third column (name, title, primary identifier)
4. **Data Columns** - Remaining columns in logical order
5. **Status Column** - Status/active indicator
6. **Audit Columns** - Created/Updated information (last columns)

### Column Types

#### 1. ID Column (Always First)
```typescript
columnHelper.accessor("entityId", {
  id: "entityId",
  header: "ID",
  cell: (info) => info.getValue(),
}),
```

#### 2. Actions Column (Always Second)
```typescript
columnHelper.display({
  id: "actions",
  header: "Actions",
  enableSorting: false,
  enableColumnFilter: false,
  cell: (info) => (
    <span className="control_btn d-flex align-items-center">
      <Button
        className="actin_btn"
        onClick={() => handleEdit(info.row.original.entityId!)}
      >
        <img className="edit" src={edit_icon} alt="" />
      </Button>
    </span>
  ),
}),
```

#### 3. Standard Data Column
```typescript
columnHelper.accessor("fieldName", {
  id: "fieldName",
  header: "Display Name",
  cell: (info) => info.getValue() || "N/A",
}),
```

#### 4. Custom Formatted Column
```typescript
columnHelper.accessor("entityName", {
  id: "entityName",
  header: "Entity Name",
  cell: (info) => (
    <div className="entity_name">
      {info.getValue() || "N/A"}
    </div>
  ),
}),
```

#### 5. Status Badge Column
```typescript
columnHelper.accessor("isActive", {
  id: "isActive",
  header: "Status",
  cell: (info) => (
    <span className={`status ${info.getValue() ? 'active' : 'inactive'}`}>
      {info.getValue() ? 'Active' : 'Inactive'}
    </span>
  ),
}),
```

#### 6. Date Column
```typescript
columnHelper.accessor("createdOn", {
  id: "createdOn",
  header: "Created On",
  cell: (info) => {
    const date = info.getValue();
    return date ? new Date(date).toLocaleDateString() : "N/A";
  },
}),
```

#### 7. Numeric Column
```typescript
columnHelper.accessor("userCount", {
  id: "userCount",
  header: "Total Users",
  cell: (info) => info.getValue() || 0,
}),
```

#### 8. Dropdown Filter Column (Geographic Data)
```typescript
columnHelper.accessor("stateName", {
  id: "stateName",
  header: "State",
  cell: (info) => info.getValue() || "N/A",
  filterFn: "includesString",
  meta: {
    filterComponent: StateCombobox,
    filterProps: {
      placeholder: "Select State",
      className: "form-select",
    },
  },
}),
```

### Numeric Columns Array

Always define numeric columns for proper filtering:

```typescript
const numericColumns = [
  'entityId',
  'userCount', 
  'activeUserCount',
  'productCount',
  'studentCount',
  'schoolCount',
  // ... other numeric fields
];
```

## Advanced Filtering System

The DataGrid component includes a sophisticated filtering system with modal-based filter interfaces:

### Filter Types by Column Type

#### String Columns
- **Equal To**: Exact match filtering
- **Not Equal To**: Exclusion filtering  
- **In Set**: Multi-value selection with checkbox interface

#### Numeric Columns
- **Equal To**: Exact numeric match
- **Not Equal To**: Numeric exclusion
- **Greater Than**: Value comparison (>)
- **Greater Than or Equal**: Value comparison (>=)
- **Less Than**: Value comparison (<)
- **Less Than or Equal**: Value comparison (<=)
- **In Set**: Multi-numeric value selection
- **Range**: Between two values (using gte + lte combination)

### Filter Features
- **Modal Interface**: Dedicated filter modal for each column with funnel icon
- **Quick Select**: Fast selection buttons for common string values
- **Autocomplete**: Datalist suggestions from unique column values
- **Multi-Select**: Checkbox interface for "In Set" filtering
- **Range Inputs**: Min/max inputs for numeric range filtering
- **Active Filter Display**: Visual badges showing applied filters with individual removal
- **Locked Filters**: Non-removable filters for pre-applied constraints
- **Filter Persistence**: Filter state maintained during session

### Filter Configuration
```typescript
// Enable numeric filtering for specific columns
const numericColumns = ['entityId', 'userCount', 'total'];

// Pre-applied filters that users cannot modify
const lockedFilters = {
  isActive: new FilterParameter({ eq: true })
};

// Initial filter state
const initialFilter = {
  status: new FilterParameter({ eq: 'active' })
};
```

## Component Structure

### Standard File Structure

```
/src/pages/[Section]/[PageName]/
├── index.tsx          # Main component
├── index.scss         # Component styles  
└── components/        # Page-specific components (if needed)
```

### Component Template

```typescript
import React, { useMemo } from "react";
import { createColumnHelper } from "@tanstack/react-table";
import { Button } from "@mui/material";
import { useNavigate } from "react-router-dom";
import { 
  [EntityClient], 
  [QueryEntityModel],
  [QueryEntityModel3],
} from "@/api/GreenOnionClient";
import edit_icon from "@/assets/icons/edit_icon.svg";
import DataGrid from "@/components/Reusable/DataGrid";
import "./index.scss";

const columnHelper = createColumnHelper<[QueryEntityModel3]>();

export default function [ComponentName]() {
  const navigate = useNavigate();

  const handleCreate = () => {
    navigate(`[create-path]`);
  };

  const handleEdit = (id: number) => {
    navigate(`[edit-path]/${id}`);
  };

  const columns = useMemo(() => [
    // Column definitions
  ], [handleEdit]);

  const numericColumns = ['entityId']; // Add numeric fields

  return (
    <section className="[page-name]-page">
      <div className="main_wrapper">
        <div className="dis_main_box min_heigh_70vh br-5">
          <div className="title f-15-700">
            <h5 className="title_text m-0">[Page Title]</h5>
          </div>

          <DataGrid<[QueryEntityModel3], [EntityClient], [QueryEntityModel]>
            title="[Page Title]"
            createButtonText="Create [Entity]"
            onCreateClick={handleCreate}
            clientClass={[EntityClient]}
            queryModelClass={[QueryEntityModel]}
            columns={columns}
            numericColumns={numericColumns}
            enableSearch={true}
            enableFilter={true}
            enableSort={true}
            enableColumnVisibility={true}
            initialPageSize={20}
            className="[page-name]-grid"
          />
        </div>
      </div>
    </section>
  );
}
```

## Styling Standards

### SCSS Structure

- **Important**: All styles should be in `rem` and not `px` were possible

Based on `/src/pages/Setting/ManageManuFacturer/index.scss`:

```scss
@use "@/App.scss" as *;

.manage-[entity]-page {
  .main_wrapper {
    padding: 20px;
  }

  .dis_main_box {
    background: #ffffff;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    padding: 24px;

    .title {
      margin-bottom: 24px;
      border-bottom: 1px solid #e9ecef;
      padding-bottom: 16px;
      display: flex;
      align-items: center;
      justify-content: space-between;

      .title_text {
        color: $text_color;
        font-size: 1.5rem;
        font-weight: 700;
      }
    }

    // Custom badge styles
    .table_box {
      .table-container {
        .table {
          tbody {
            td {
              // Status badges
              .status {
                padding: 4px 8px;
                border-radius: 12px;
                font-size: 11px;
                font-weight: 600;
                text-transform: uppercase;

                &.active {
                  background: #d4edda;
                  color: #155724;
                }

                &.inactive {
                  background: #f8d7da;
                  color: #721c24;
                }
              }

              // Custom entity-specific badges
              .entity-specific-badge {
                padding: 4px 8px;
                border-radius: 12px;
                font-size: 11px;
                font-weight: 600;

                &.positive-state {
                  background: #cce5ff;
                  color: #004085;
                }

                &.neutral-state {
                  background: #f8f9fa;
                  color: #6c757d;
                }
              }
            }
          }
        }
      }
    }
  }

  // Responsive design
  @media (max-width: 768px) {
    .main_wrapper {
      padding: 12px;
    }

    .dis_main_box {
      padding: 16px;

      .title {
        flex-direction: column;
        gap: 16px;
        align-items: stretch;
      }
    }
  }
}
```

### Badge Color System

```scss
// Status badges (universal)
.status {
  &.active {
    background: #d4edda;
    color: #155724;
  }
  
  &.inactive {
    background: #f8d7da;
    color: #721c24;
  }
}

// Informational badges
.info-badge {
  &.has-data {
    background: #cce5ff;
    color: #004085;
  }
  
  &.no-data {
    background: #f8f9fa;
    color: #6c757d;
  }
  
  &.sync-enabled {
    background: #d1ecf1;
    color: #0c5460;
  }
}
```

## TypeScript Patterns

### Generic Type Parameters

```typescript
// Standard pattern for DataGrid
<DataGrid<TData, TClient, TQueryModel>
  // Props...
/>

// Where:
// TData = QueryEntityModel3 (search result type)
// TClient = EntityClient (API client type)  
// TQueryModel = QueryEntityModel (query parameters type)
```

### Column Helper Typing

```typescript
// Create typed column helper
const columnHelper = createColumnHelper<QueryEntityModel3>();

// Use with proper accessor typing
columnHelper.accessor("fieldName", {
  // TypeScript will enforce fieldName exists on QueryEntityModel3
});
```

### Handler Function Signatures

```typescript
// Standard handlers
const handleCreate = (): void => {
  navigate(`[create-path]`);
};

const handleEdit = (id: number): void => {
  navigate(`[edit-path]/${id}`);
};

const handleView = (id: number): void => {
  navigate(`[view-path]/${id}`);
};
```

## Best Practices

### 1. Component Organization

```typescript
// Import order
import React, { useMemo } from "react";           // React imports
import { createColumnHelper } from "@tanstack/react-table"; // Third-party
import { Button } from "@mui/material";            // UI library
import { useNavigate } from "react-router-dom";    // React Router
import { /* API types */ } from "@/api/GreenOnionClient"; // Generated API
import edit_icon from "@/assets/icons/edit_icon.svg"; // Assets
import DataGrid from "@/components/Reusable/DataGrid"; // Components
import "./index.scss";                             // Styles
```

### 2. Performance Optimization

```typescript
// Always wrap columns in useMemo
const columns = useMemo(
  () => [
    // Column definitions
  ],
  [handleEdit, handleView] // Include all handlers in dependencies
);

// Define numeric columns as constant
const numericColumns = ['entityId', 'count', 'total'];
```

### 3. Error Handling

```typescript
// Handle missing data gracefully
cell: (info) => info.getValue() || "N/A"

// Handle date formatting safely
cell: (info) => {
  const date = info.getValue();
  return date ? new Date(date).toLocaleDateString() : "N/A";
}
```

### 4. Accessibility

```typescript
// Always include alt text for images
<img className="edit" src={edit_icon} alt="Edit" />

// Use semantic HTML in cells
<div className="entity_name" role="cell">
  {info.getValue() || "N/A"}
</div>
```

## Examples

### Complete Implementation Example

```typescript
import React, { useMemo } from "react";
import { createColumnHelper } from "@tanstack/react-table";
import { Button } from "@mui/material";
import { useNavigate } from "react-router-dom";
import { 
  UserClient, 
  QueryUserModel,
  QueryUserModel3,
} from "@/api/GreenOnionClient";
import edit_icon from "@/assets/icons/edit_icon.svg";
import view_icon from "@/assets/icons/sunglass.svg";
import DataGrid from "@/components/Reusable/DataGrid";
import "./index.scss";

const columnHelper = createColumnHelper<QueryUserModel3>();

export default function Users() {
  const navigate = useNavigate();

  const handleCreate = () => {
    navigate(`create`);
  };

  const handleView = (id: number) => {
    navigate(`view/${id}`);
  };

  const handleEdit = (id: number) => {
    navigate(`edit/${id}`);
  };

  const columns = useMemo(
    () => [
      columnHelper.accessor("userId", {
        id: "userId",
        header: "ID",
        cell: (info) => info.getValue(),
      }),
      columnHelper.display({
        id: "actions",
        header: "Actions",
        enableSorting: false,
        enableColumnFilter: false,
        cell: (info) => (
          <span className="control_btn d-flex align-items-center">
            <Button
              className="actin_btn"
              onClick={() => handleView(info.row.original.userId!)}
            >
              <img className="view" src={view_icon} alt="View" />
            </Button>
            <Button
              className="actin_btn"
              onClick={() => handleEdit(info.row.original.userId!)}
            >
              <img className="edit" src={edit_icon} alt="Edit" />
            </Button>
          </span>
        ),
      }),
      columnHelper.accessor("firstName", {
        id: "firstName",
        header: "First Name",
        cell: (info) => info.getValue() || "N/A",
      }),
      columnHelper.accessor("lastName", {
        id: "lastName",
        header: "Last Name",
        cell: (info) => info.getValue() || "N/A",
      }),
      columnHelper.accessor("email", {
        id: "email",
        header: "Email",
        cell: (info) => info.getValue() || "N/A",
      }),
      columnHelper.accessor("isActive", {
        id: "isActive",
        header: "Status",
        cell: (info) => (
          <span className={`status ${info.getValue() ? 'active' : 'inactive'}`}>
            {info.getValue() ? 'Active' : 'Inactive'}
          </span>
        ),
      }),
    ],
    [handleEdit, handleView]
  );

  const numericColumns = ['userId'];

  return (
    <section className="users-page">
      <div className="main_wrapper">
        <div className="dis_main_box min_heigh_70vh br-5">
          <div className="title f-15-700">
            <h5 className="title_text m-0">Users List</h5>
          </div>

          <DataGrid<QueryUserModel3, UserClient, QueryUserModel>
            title="Users List"
            createButtonText="Create User"
            onCreateClick={handleCreate}
            clientClass={UserClient}
            queryModelClass={QueryUserModel}
            columns={columns}
            numericColumns={numericColumns}
            enableSearch={true}
            enableFilter={true}
            enableSort={true}
            enableColumnVisibility={true}
            initialPageSize={20}
            className="users-grid"
          />
        </div>
      </div>
    </section>
  );
}
```

## Migration Checklist

When updating existing DataGrid implementations to match this standard:

### ✅ Required Changes

- [ ] **Update API Pattern**: Replace `apiClient`/`queryMethod` with `clientClass`/`queryModelClass`
- [ ] **Standardize Props**: Use exact prop names from gold standard
- [ ] **Add Missing Features**: Enable search, filter, sort, and column visibility
- [ ] **Fix TypeScript Generics**: Use proper 3-generic pattern `<TData, TClient, TQueryModel>`
- [ ] **Standardize Page Size**: Use `initialPageSize={20}`
- [ ] **Add CSS Class**: Include component-specific `className="[page-name]-grid"`
- [ ] **Update Imports**: Ensure proper import order and types

### ✅ Column Updates

- [ ] **Column Helper**: Use `createColumnHelper<TData>()`
- [ ] **useMemo Wrapper**: Wrap columns in `useMemo` with proper dependencies
- [ ] **ID Column First**: Always start with entity ID column
- [ ] **Actions Column Second**: Standardize action column format
- [ ] **Consistent Cell Rendering**: Use `|| "N/A"` for missing data
- [ ] **Numeric Columns Array**: Define array of numeric column IDs

### ✅ Styling Updates

- [ ] **Page Class**: Use `.manage-[entity]-page` or `.[section]-page` pattern
- [ ] **Import Base Styles**: `@use "@/App.scss" as *;`
- [ ] **Standard Badge Classes**: Use `.status.active/.inactive` pattern
- [ ] **Responsive Design**: Include mobile breakpoints
- [ ] **Remove Material-UI Grid**: Replace with custom DataGrid if using MUI

### ✅ Code Quality

- [ ] **Remove Unused Code**: Clean up legacy filtering/pagination code
- [ ] **Handler Standardization**: Use consistent handler naming and signatures
- [ ] **Import Cleanup**: Remove unused imports
- [ ] **TypeScript Compliance**: Fix any TypeScript errors
- [ ] **Navigation Consistency**: Standardize route patterns

## Dropdown Component Standards

### Overview

The Green Onion application uses standardized dropdown components for all form selects, ensuring type safety, performance, and consistent user experience across the application.

### Available Dropdown Components

- **CategoryCombobox** - Product categories (active only)
- **SubCategoryCombobox** - Product sub-categories (filtered by category, active only)
- **ManufacturerCombobox** - Manufacturer selection (active only)
- **AllergenCombobox** - Allergen selection
- **DistributorCombobox** - Distributor selection (active only)
- **SchoolDistrictCombobox** - School district selection (with role-based filtering, active only)
- **IngredientCombobox** - Ingredient selection
- **IOCCategoryCombobox** - IOC category selection
- **RoleCombobox** - User role selection (active only)
- **StateCombobox** - US State selection with caching
- **StorageTypeCombobox** - Storage type selection

### Standard Implementation Pattern

**⚠️ CRITICAL: Form State Management Requirements**
- **MANDATORY**: Use React Hook Form as single source of truth
- **REQUIRED**: Pass `value` props to all dropdowns for controlled behavior
- **FORBIDDEN**: Multiple state management systems (useState + useForm)

```typescript
import React, { useCallback } from 'react';
import { useForm } from 'react-hook-form';
import { CategoryCombobox, ManufacturerCombobox, IngredientCombobox, RoleCombobox, StateCombobox, StorageTypeCombobox } from '@/components/ui/controls';

interface MyFormData {
  categoryId: number;
  manufacturerId: number;
  ingredientId: number;
  stateId: number;
}

export default function MyFormComponent() {
  const { register, handleSubmit, watch, setValue, reset, formState: { errors } } = useForm<MyFormData>();
  
  // ✅ REQUIRED: Watch form values for controlled components
  const watchedValues = watch();
  
  // ✅ REQUIRED: Handle dependent field resets properly
  const handleCategoryChange = useCallback((value: number) => {
    setValue('subCategoryId', 0); // Reset dependent field only
  }, [setValue]);

  const onSubmit = (data: MyFormData) => {
    console.log('Form submitted:', data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div className="form-group">
        <label htmlFor="categoryId">Category *</label>
        <CategoryCombobox<MyFormData>
          value={watchedValues.categoryId}  {/* ✅ REQUIRED: Controlled value */}
          register={register}
          name="categoryId"
          className="form-select"
          onChange={handleCategoryChange}   {/* ✅ REQUIRED: Only for special logic */}
          required={true}
          validationOptions={{
            required: "Category is required"
          }}
        />
        {errors.categoryId && (
          <span className="error-message">{errors.categoryId.message}</span>
        )}
      </div>

      <div className="form-group">
        <label htmlFor="manufacturerId">Manufacturer</label>
        <ManufacturerCombobox<MyFormData>
          value={watchedValues.manufacturerId}  {/* ✅ REQUIRED: Controlled value */}
          register={register}
          name="manufacturerId"
          className="form-select"
          onChange={() => {}}                   {/* ✅ REQUIRED: No special logic needed */}
          placeholder="Select Manufacturer"
        />
      </div>

      <div className="form-group">
        <label htmlFor="ingredientId">Ingredient</label>
        <IngredientCombobox<MyFormData>
          value={watchedValues.ingredientId}     {/* ✅ REQUIRED: Controlled value */}
          register={register}
          name="ingredientId"
          className="form-select"
          onChange={() => {}}                   {/* ✅ REQUIRED: No special logic needed */}
          placeholder="Select Ingredient"
        />
      </div>

      <div className="form-group">
        <label htmlFor="stateId">State *</label>
        <StateCombobox<MyFormData>
          value={watchedValues.stateId}         {/* ✅ REQUIRED: Controlled value */}
          register={register}
          name="stateId"
          className="form-select"
          onChange={() => {}}                   {/* ✅ REQUIRED: No special logic needed */}
          required={true}
          validationOptions={{
            required: "State is required"
          }}
        />
        {errors.stateId && (
          <span className="error-message">{errors.stateId.message}</span>
        )}
      </div>
      
      <div className="form-actions">
        <button type="submit">Submit</button>
        <button type="button" onClick={() => reset()}>Reset</button>
      </div>
    </form>
  );
}
```

### ⚠️ CRITICAL: Anti-Patterns to Avoid

#### ❌ FORBIDDEN: Multiple State Management Systems
```typescript
// ❌ NEVER DO THIS: Conflicting state management
const [formValues, setFormValues] = useState({});  // Custom state
const { register, setValue } = useForm();          // React Hook Form

const handleChange = (field, value) => {
  setFormValues(prev => ({ ...prev, [field]: value })); // Conflicts!
  setValue(field, value);                                // Conflicts!
};
```

#### ❌ FORBIDDEN: Missing Value Props
```typescript
// ❌ NEVER DO THIS: Dropdown without controlled value
<CategoryCombobox
  register={register}
  name="categoryId"
  onChange={(value) => console.log(value)}
  // Missing: value={watchedValues.categoryId}
/>
```

#### ❌ FORBIDDEN: Manual setValue in onChange
```typescript
// ❌ NEVER DO THIS: Manual setValue conflicts with register
<CategoryCombobox
  onChange={(value) => {
    setValue('categoryId', value); // Conflicts with register's onChange!
  }}
/>
```

### ✅ REQUIRED: Proper Implementation Patterns

#### Form State Management Requirements
- **Single Source of Truth**: React Hook Form only
- **Controlled Components**: All dropdowns must have `value` props
- **Register Integration**: Use `register` for form field management
- **Dependent Resets**: Use `setValue` only for resetting dependent fields

### Key Features

- **Generic TypeScript Support**: All dropdowns support form value typing
- **React Hook Form Integration**: Full validation and error handling support
- **Cached API Calls**: 30-minute TTL caching for optimal performance
- **Server-Side Sorting**: All data sorted alphabetically on the server
- **Active Record Filtering**: All dropdowns automatically filter for `isActive = true` when ModelFilter supports IsActive property
- **Loading States**: Built-in loading indicators during data fetch
- **Error Handling**: Graceful error handling with console logging
- **Type Safety**: Comprehensive TypeScript interfaces with JSDoc documentation
- **Form State Persistence**: Values maintain selection state across interactions

### Dependent Dropdown Pattern

For dropdowns that depend on other selections (like SubCategory depending on Category):

```typescript
interface ProductFormData {
  categoryId: number;
  subCategoryId: number;
}

export default function ProductForm() {
  const { register, handleSubmit, setValue, watch, reset } = useForm<ProductFormData>();
  const watchedValues = watch(); // ✅ REQUIRED: Watch all values
  
  // ✅ REQUIRED: Handle dependent field resets
  const handleCategoryChange = useCallback((value: number) => {
    setValue('subCategoryId', 0); // Reset dependent field only
  }, [setValue]);

  const onSubmit = (data: ProductFormData) => {
    console.log('Form submitted:', data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <CategoryCombobox<ProductFormData>
        value={watchedValues.categoryId}      {/* ✅ REQUIRED: Controlled value */}
        register={register}
        name="categoryId"
        onChange={handleCategoryChange}       {/* ✅ REQUIRED: Reset dependent only */}
        required={true}
      />

      <SubCategoryCombobox<ProductFormData>
        value={watchedValues.subCategoryId}   {/* ✅ REQUIRED: Controlled value */}
        register={register}
        name="subCategoryId"
        categoryId={watchedValues.categoryId} {/* Filter by selected category */}
        disabled={!watchedValues.categoryId}  {/* Disable until category is selected */}
        onChange={() => {}}                   {/* ✅ REQUIRED: No special logic needed */}
        placeholder={watchedValues.categoryId ? "Select Sub Category" : "Select Category first"}
      />
    </form>
  );
}
```

### Styling Standards

```scss
// Standard form group styling for dropdowns
.form-group {
  margin-bottom: 1.5rem;

  label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: 600;
    color: #2c3e50;

    &.required::after {
      content: " *";
      color: #e74c3c;
    }
  }

  .form-select {
    width: 100%;
    padding: 0.75rem;
    border: 0.0625rem solid #ddd;
    border-radius: 0.375rem;
    font-size: 1rem;
    background-color: #fff;
    transition: border-color 0.15s ease-in-out;

    &:focus {
      outline: none;
      border-color: #007bff;
      box-shadow: 0 0 0 0.125rem rgba(0, 123, 255, 0.25);
    }

    &:disabled {
      background-color: #f8f9fa;
      opacity: 0.65;
      cursor: not-allowed;
    }

    option {
      padding: 0.5rem;
    }
  }
}
```

### Performance Considerations

- **Cached Data**: All dropdown data is cached for 30 minutes to reduce API calls
- **Server-Side Sorting**: Data is sorted on the server to reduce client-side processing
- **Lazy Loading**: Dropdown data is only loaded when component mounts
- **Memoization**: Components use `useCallback` for optimal performance

### IsActive Filtering Pattern

#### Overview
All dropdown components automatically filter for active records when the underlying `ModelFilter` class supports an `isActive` property. This ensures that users only see active, available options in dropdown selections.

#### Implementation Pattern
```typescript
// ✅ REQUIRED: IsActive filtering implementation
const fetchData = useCallback(async (): Promise<DropdownOption[]> => {
  const client = new EntityClient();
  const res = await client.query({
    body: new QueryEntityModelSearchQuery({
      currentPage: 0,
      pageSize: 1000,
      orderBy: new QueryEntityModelOrderBy({
        entityName: OrderDirections.Asc
      }),
      filter: new QueryEntityModelFilter({
        isActive: new FilterParameter({ eq: true })  // Filter for active records only
      })
    })
  });

  if (res && res.rows) {
    return res.rows.map((entity: QueryEntityModel) => ({
      value: entity.entityId!,
      label: entity.entityName!
    }));
  }
  
  return [];
}, []);
```

#### Supported Dropdowns with IsActive Filtering
- **CategoryCombobox**: Filters `QueryCategoryModelFilter.isActive`
- **DistributorCombobox**: Filters `QueryDistributorModelFilter.isActive`
- **ManufacturerCombobox**: Filters `QueryManufacturerModelFilter.isActive`
- **RoleCombobox**: Filters `QueryRoleModelFilter.isActive`
- **SchoolDistrictCombobox**: Filters `QuerySchoolDistrictModelFilter.isActive` (combined with role-based filtering)
- **SubCategoryCombobox**: Filters `QuerySubCategoryModelFilter.isActive` (combined with category filtering)

#### Complex Filtering Examples
```typescript
// ✅ SchoolDistrictCombobox: Combines IsActive with role-based filtering
const filterCriteria: any = {
  isActive: new FilterParameter({ eq: true })
};

if (profile?.roleId === 2) {
  filterCriteria.schoolDistrictId = new FilterParameter({ eq: profile.schoolDistrictId });
}

queryModel.filter = new QuerySchoolDistrictModelFilter(filterCriteria);

// ✅ SubCategoryCombobox: Combines IsActive with category filtering
const filterCriteria: any = {
  isActive: new FilterParameter({ eq: true })
};

if (categoryId && categoryId > 0) {
  filterCriteria.categoryId = new FilterParameter({ eq: categoryId });
}

queryModel.filter = new QuerySubCategoryModelFilter(filterCriteria);
```

#### Documentation Updates
All affected dropdown components include updated JSDoc comments:
- `@description` mentions "active records only"
- Function documentation specifies "Automatically filters for isActive = true"

### Migration from Legacy Dropdowns

When migrating from manual dropdown implementations:

1. **Replace manual state management** with reusable dropdown components
2. **Remove client-side sorting** - server handles all sorting
3. **Add TypeScript generic support** for form value typing
4. **Implement validation options** using React Hook Form patterns
5. **Update styling** to use consistent form-select classes
6. **Verify IsActive filtering** - ensure dropdowns show only active records

### ⚠️ CRITICAL: Form State Management Migration Checklist

#### Required Changes for Existing Forms
- [ ] **Remove Custom State**: Delete any `useState` for form data management
- [ ] **Add Value Props**: All dropdowns must have `value={watchedValues.fieldId}`
- [ ] **Watch Form Values**: Use `const watchedValues = watch()` for controlled components
- [ ] **Simplify onChange**: Remove manual `setValue` calls except for dependent resets
- [ ] **Use Form Reset**: Replace manual clearing with `reset()` method
- [ ] **Add Error Display**: Include validation error display components
- [ ] **Test State Persistence**: Verify selections persist across interactions

#### Common Migration Issues
- **Lost Selections**: Missing `value` props cause dropdowns to not show current selection
- **State Conflicts**: Multiple state systems cause form values to reset unexpectedly
- **Validation Errors**: Form validation breaks after improper state management changes
- **Performance Issues**: Unnecessary re-renders from conflicting watch/setValue patterns

#### Before/After Migration Pattern
```typescript
// ❌ BEFORE: Problematic implementation
const [selectedValues, setSelectedValues] = useState({});
const { register, setValue } = useForm();

const updateValue = (field, value) => {
  setSelectedValues(prev => ({ ...prev, [field]: value }));
  setValue(field, value);
};

<CategoryCombobox onChange={(value) => updateValue('categoryId', value)} />

// ✅ AFTER: Correct implementation  
const { register, watch, setValue } = useForm();
const watchedValues = watch();

const handleCategoryChange = useCallback((value) => {
  setValue('subCategoryId', 0); // Only reset dependent
}, [setValue]);

<CategoryCombobox 
  value={watchedValues.categoryId}
  register={register}
  onChange={handleCategoryChange}
/>
```

## Management Pattern Implementation Guide

### Implementing Full CRUD Pattern

#### 1. Component Structure
```typescript
export default function ManageEntity() {
  const navigate = useNavigate();

  const handleCreate = () => {
    navigate('/settings/entities/edit'); // Unified edit route
  };

  const handleEdit = (id: number) => {
    navigate(`/settings/entities/edit/${id}`);
  };

  // Column definitions with actions column including edit button
  const columns = useMemo(() => [
    // ID column
    // Actions column with edit button
    // Data columns
  ], [handleEdit]);

  return (
    <section className="manage-entity-page">
      <div className="main_wrapper">
        <div className="dis_main_box min_heigh_70vh br-5">
          <div className="title f-15-700">
            <h5 className="title_text m-0">Entity Management</h5>
          </div>

          <DataGrid
            createButtonText="Create Entity"
            onCreateClick={handleCreate}
            // ... other props
          />
        </div>
      </div>
    </section>
  );
}
```

#### 2. Required Props for Full CRUD
- `createButtonText` - Always "Create [Entity]"
- `onCreateClick` - Navigation to unified edit route
- Single page title "[Entity] Management"
- Only edit action in table rows
- **⚠️ IMPORTANT**: Only ONE create button via DataGrid props (no additional buttons in page header)

### Implementing Edit-Only Pattern

#### 1. Component Structure
```typescript
export default function Users() {
  const navigate = useNavigate();

  const handleEdit = (id: number) => {
    navigate(`/settings/users/edit/${id}`);
  };

  // Column definitions with actions column including only edit button
  const columns = useMemo(() => [
    // ID column
    // Actions column with edit button only
    // Data columns
  ], [handleEdit]);

  return (
    <section className="users-page">
      <div className="main_wrapper">
        <div className="dis_main_box min_heigh_70vh br-5">
          <div className="title f-15-700">
            <h5 className="title_text m-0">User Management</h5>
          </div>

          {/* Note: No createButtonText or onCreateClick */}
          <DataGrid
            // ... props without create functionality
          />
        </div>
      </div>
    </section>
  );
}
```

#### 2. Required Props for Edit-Only
- No `createButtonText` or `onCreateClick`
- Single page title "[Entity] Management"
- Only edit action in table rows
- Note in code explaining auto-creation process

### Pattern Decision Matrix

| Entity Type | Pattern | Reason | Examples |
|-------------|---------|--------|----------|
| Manufacturer | Full CRUD | Manual admin creation required | ManageManuFacturer |
| District | Full CRUD | Manual admin setup required | ManageDistrict |
| Allergen | Full CRUD | Manual data entry required | ManageAllergens |
| Ingredient | Full CRUD | Manual configuration required | ManageIngredients |
| User | Edit-Only | Auto-created during registration | Users |

### Business Logic Considerations

#### Choose Full CRUD When:
- Entity requires manual administrative creation
- Business workflow supports create operations
- Data integrity requires admin oversight for creation
- Complex validation rules for new entities

#### Choose Edit-Only When:
- Entity auto-created through external processes
- Manual creation would conflict with business logic
- Creation happens during user registration/onboarding
- System-generated entities with minimal admin oversight

## Conclusion

Following this style guide ensures:

1. **Consistency** across all DataGrid implementations
2. **Maintainability** through standardized patterns
3. **Type Safety** with proper TypeScript usage
4. **Performance** via optimized React patterns
5. **User Experience** through consistent UI/UX
6. **Developer Experience** through clear conventions
7. **Business Logic Alignment** through appropriate management patterns

All new DataGrid implementations must follow this guide, and existing implementations should be migrated according to the checklist above. The choice between Full CRUD and Edit-Only patterns should be based on the underlying business logic and entity creation workflows.