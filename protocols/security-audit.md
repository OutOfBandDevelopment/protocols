# Security Audit Protocol

## Document Information

| Field | Value |
|-------|-------|
| Version | 1.0 |
| Last Updated | 2025-12-31 |
| Status | Active |

---

## Overview

This protocol defines the steps for conducting a security audit of the GreenOnion API. The audit ensures that all endpoints and services are properly secured according to the application's security model.

### Key Security Principles

1. **Secure by Default** - Global AuthorizeFilter requires authentication on ALL controllers
2. **Explicit Opt-Out** - Only `[AllowAnonymous]` can make endpoints public
3. **Role-Based Access** - `[ApplicationRole(...)]` restricts to specific roles
4. **Data Scoping** - Users only see data within their organizational context
5. **Service-Layer Authorization** - Complex authorization logic lives in services

---

## Pre-Audit Checklist

Before starting the audit, verify these foundational elements:

### 1. Verify Secure-by-Default Configuration

```bash
# Check that global AuthorizeFilter is registered
grep -r "AuthorizeFilter" --include="*.cs" src/Common/
```

**Expected location**: `src/Common/Eliassen.Common.AspNetCore/Mvc/ServiceCollectionExtensions.cs`

**Expected pattern**:
```csharp
var authorizationPolicy = new AuthorizationPolicyBuilder()
    .RequireAuthenticatedUser()
    .AddRequirements(new UserAuthorizationRequirement(requireApplicationUserId))
    .Build();

services.AddControllers(options =>
    options.Filters.Add(new AuthorizeFilter(authorizationPolicy)));
```

### 2. Verify UserAuthorizationHandler is Registered

```bash
grep -r "UserAuthorizationHandler" --include="*.cs" src/
```

**Expected**: Handler registered as `IAuthorizationHandler`

---

## Audit Steps

### Step 1: Controller Security Attributes

Run these searches to identify controller security status:

```bash
# Find all controllers
find src -name "*Controller.cs" -type f

# Find controllers with [Authorize] attribute
grep -l "\[Authorize\]" src/**/*Controller.cs

# Find controllers with [AllowAnonymous] attribute
grep -l "\[AllowAnonymous\]" src/**/*Controller.cs

# Find actions with [ApplicationRole] attribute
grep -rn "\[ApplicationRole" --include="*Controller.cs" src/
```

#### Security Attribute Matrix

For each controller, document:

| Controller | Class [Authorize] | Has [AllowAnonymous] | Role-Restricted Actions | Notes |
|------------|-------------------|----------------------|------------------------|-------|
| ProductController | Yes | No | Save, BulkAllocate | Correct |
| UserController | Yes | No | Save (service-layer) | Intentional |
| ... | ... | ... | ... | ... |

#### Verification Rules

| Scenario | Expected | Action if Missing |
|----------|----------|-------------------|
| Controller has no attributes | Protected (global filter) | Add `[Authorize]` for documentation |
| Controller has `[Authorize]` | Protected | None - correct |
| Controller has `[AllowAnonymous]` | Public | Verify intentional, document reason |
| Action has `[ApplicationRole]` | Role-restricted | Verify roles are appropriate |
| Action has `[AllowAnonymous]` | Public action on protected controller | Verify intentional |

### Step 2: Service-Layer Authorization

Some authorization must occur at the service layer. Audit these patterns:

```bash
# Find service methods that check user context
grep -rn "GetSession\(\)" --include="*Service.cs" src/

# Find permission checks in services
grep -rn "ApplicationRoles\." --include="*Service.cs" src/

# Find data scoping patterns
grep -rn "SchoolDistrictId\|CooperativeId" --include="*Service.cs" src/
```

#### Service Authorization Checklist

| Service | Has User Context | Data Scoping | Role Checks | Notes |
|---------|-----------------|--------------|-------------|-------|
| ProductService | Yes | District/Coop | SuperAdmin for Save | Correct |
| UserService | Yes | Self + Admin | Edit own or admin | Intentional |
| ... | ... | ... | ... | ... |

### Step 3: Data Scoping Audit

Verify that queries respect organizational boundaries:

```bash
# Find repository query methods
grep -rn "IQueryable\|\.Query\(\)" --include="*Repository.cs" src/

# Check for user context in queries
grep -rn "user\?.SchoolDistrictId\|user\?.CooperativeId" --include="*Repository.cs" src/
```

#### Data Scoping Rules

| User Role | Should See | Scoping Field |
|-----------|-----------|---------------|
| SuperAdmin | All data | None (unfiltered) |
| CooperativeAdmin | Cooperative + member districts | CooperativeId or SchoolDistrictId IN members |
| DistrictAdmin | Own district only | SchoolDistrictId |
| DistrictUser | Own district only | SchoolDistrictId |
| ManufacturerUser | Own manufacturer only | ManufacturerId |

### Step 4: Endpoint Security Classification

Classify all endpoints by security level:

#### Public Endpoints (Should be minimal)

```bash
grep -rn "\[AllowAnonymous\]" --include="*Controller.cs" src/
```

**Valid public endpoints**:
- Health checks
- OpenAPI/Swagger documentation
- Authentication callbacks (if any)

**Flag for review**: Any business data endpoints marked public

#### Role-Restricted Endpoints

```bash
grep -rn "\[ApplicationRole\(" --include="*Controller.cs" src/
```

**Expected patterns**:
| Operation Type | Expected Roles |
|---------------|----------------|
| Query/Get | Any authenticated (or role-specific) |
| Save (create/update) | SuperAdmin, or contextual (own data) |
| Delete | SuperAdmin |
| Bulk operations | SuperAdmin, or elevated roles |
| Admin functions | SuperAdmin |

### Step 5: Bulk Action Security

Verify bulk actions have appropriate protection:

```bash
# Find all bulk action endpoints
grep -rn "public.*Task.*Bulk" --include="*Controller.cs" src/

# Verify they have role restrictions
grep -B5 "public.*Task.*Bulk" --include="*Controller.cs" src/ | grep -E "ApplicationRole|Authorize"
```

**Required for bulk actions**:
- `[ApplicationRole(...)]` with appropriate roles
- `[BulkActionTarget(...)]` for OpenAPI integration
- Confirmation required (`RequiresConfirmation = true`) for destructive actions

### Step 6: Sensitive Data Protection

Check for proper handling of sensitive fields:

```bash
# Find password/secret handling
grep -rn "Password\|Secret\|Token\|ApiKey" --include="*.cs" src/

# Check for proper exclusion from exports
grep -rn "\[ExportExclude\]" --include="*.cs" src/

# Check for hidden fields
grep -rn "\[HiddenField\]\|\[Hidden\]" --include="*.cs" src/
```

---

## Audit Report Template

```markdown
# Security Audit Report

**Date**: YYYY-MM-DD
**Auditor**: [Name/Claude]
**Scope**: [Full/Partial - specify areas]

## Executive Summary

- Total Controllers Audited: X
- Total Services Audited: X
- Issues Found: X (Critical: X, High: X, Medium: X, Low: X)

## Secure-by-Default Status

- [ ] Global AuthorizeFilter configured
- [ ] UserAuthorizationHandler registered
- [ ] Default policy requires authentication

## Findings

### Critical Issues
[List any critical security issues]

### High Priority Issues
[List high priority issues]

### Medium Priority Issues
[List medium priority issues]

### Low Priority/Recommendations
[List low priority items and recommendations]

## Controller Audit Results

| Controller | Status | Issues | Notes |
|------------|--------|--------|-------|
| ... | ... | ... | ... |

## Service Audit Results

| Service | Data Scoping | Role Checks | Issues |
|---------|--------------|-------------|--------|
| ... | ... | ... | ... |

## Recommendations

1. [Recommendation 1]
2. [Recommendation 2]
...

## Sign-off

- [ ] All critical issues addressed
- [ ] High priority issues have remediation plan
- [ ] Documentation updated
```

---

## Common Security Anti-Patterns

### DO NOT

1. **Skip authorization on new controllers** - Even with global filter, add `[Authorize]` for clarity
2. **Use `[AllowAnonymous]` on data endpoints** - Business data should always be protected
3. **Forget service-layer authorization** - Controller attributes aren't enough for complex rules
4. **Ignore data scoping** - Always filter by user context in repositories
5. **Return sensitive data in queries** - Use `[ExportExclude]`, `[HiddenField]` appropriately
6. **Allow bulk operations without confirmation** - Set `RequiresConfirmation = true`

### DO

1. **Add `[Authorize]` to all controllers** - Documents intent even though global filter protects
2. **Use `[ApplicationRole]` for elevated operations** - Save, Delete, Admin functions
3. **Implement data scoping in repositories** - Filter by SchoolDistrictId, CooperativeId, etc.
4. **Validate user context in services** - Check ownership before modifications
5. **Log security-relevant operations** - Audit trail for admin actions
6. **Review `[AllowAnonymous]` usage** - Should be rare and documented

---

## Automated Checks

Add these to CI/CD pipeline:

```bash
#!/bin/bash
# security-check.sh

echo "=== Security Audit Checks ==="

# Check for controllers without [Authorize]
echo "Checking for undocumented controllers..."
CONTROLLERS=$(find src -name "*Controller.cs" -exec grep -L "\[Authorize\]" {} \;)
if [ -n "$CONTROLLERS" ]; then
    echo "WARNING: Controllers without [Authorize] attribute (relying on global filter):"
    echo "$CONTROLLERS"
fi

# Check for suspicious [AllowAnonymous] usage
echo "Checking [AllowAnonymous] usage..."
grep -rn "\[AllowAnonymous\]" --include="*Controller.cs" src/

# Check bulk actions have role restrictions
echo "Checking bulk action security..."
for file in $(grep -l "Bulk" src/**/*Controller.cs 2>/dev/null); do
    echo "Checking $file..."
    grep -B10 "public.*Task.*Bulk" "$file" | grep -q "ApplicationRole" || echo "WARNING: Bulk action without role restriction in $file"
done

echo "=== Security Audit Complete ==="
```

---

## Related Documents

| Document | Description |
|----------|-------------|
| [security-model.md](../../docs/architecture/security-model.md) | Security architecture |
| [design-decisions.md](../../docs/architecture/design-decisions.md) | ADR-000: Secure by Default |
| [bulk-actions-design.md](../../docs/architecture/bulk-actions-design.md) | Bulk operation security |

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-12-31 | Initial protocol |
