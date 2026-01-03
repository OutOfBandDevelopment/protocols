# Build Validation Protocol

**Purpose**: Protocol for validating code changes by running .NET builds and capturing structured output for analysis.

**Version**: 1.0
**Created**: 2025-10-16
**Status**: Active

---

## Overview

This protocol defines how to run .NET builds after code changes and analyze the output to verify:
- Compilation succeeds
- No new warnings introduced
- Code fix doesn't break existing functionality
- Build output is structured for automated analysis

## Build Script

### Location
`/current/src/scripts/validate-build.sh`

### Usage
```bash
# Validate single project
./scripts/validate-build.sh <project-path>

# Example
./scripts/validate-build.sh InternalTools/CadLink.Common/CadLink.Common.csproj
```

### Output Format
The script generates JSON output to `build-results.json`:

```json
{
  "timestamp": "2025-10-16T10:30:00Z",
  "project": "InternalTools/CadLink.Common/CadLink.Common.csproj",
  "success": true,
  "duration_seconds": 12.5,
  "warnings": [],
  "errors": [],
  "summary": {
    "total_warnings": 0,
    "total_errors": 0,
    "build_succeeded": true
  }
}
```

## Validation Steps

### 1. Pre-Build Check
- Verify project file exists
- Check for syntax errors in modified files
- Ensure dependencies are available

### 2. Build Execution
- Run `dotnet build --configuration Release`
- Capture stdout and stderr
- Record build duration
- Parse warnings and errors

### 3. Post-Build Analysis
- Compare warning count to baseline
- Check for new errors
- Validate build artifacts exist
- Generate structured report

### 4. Result Interpretation

**Success Criteria**:
- Build exit code = 0
- No new errors introduced
- Warning count ≤ baseline (or documented increase)

**Failure Criteria**:
- Build exit code ≠ 0
- Compilation errors present
- Critical warnings introduced

## Output Analysis

### Parsing Build Output

**Warning Pattern**:
```
<file>(<line>,<col>): warning <code>: <message>
```

**Error Pattern**:
```
<file>(<line>,<col>): error <code>: <message>
```

### Common Warning Codes
- **CS0168**: Variable declared but never used
- **CS0219**: Variable assigned but never used
- **CS0618**: Member is obsolete
- **CS1998**: Async method lacks await

### Common Error Codes
- **CS0103**: Name does not exist in context
- **CS0246**: Type or namespace not found
- **CS1501**: No overload matches method signature
- **CS1061**: Type does not contain definition

## Integration with Remediation Workflow

### Task Completion Checklist
When completing a P0/P1/P2/P3 task:

1. **Apply Fix**: Edit source code
2. **Validate Build**: Run `validate-build.sh`
3. **Analyze Results**: Check `build-results.json`
4. **Update Tracker**: Mark task complete in REMEDIATION-TRACKER.md
5. **Document**: Add notes about any warnings/issues

### Example Workflow

```bash
# 1. Apply fix (using Edit tool)
# ... code changes ...

# 2. Validate build
./scripts/validate-build.sh InternalTools/CadLink.Common/CadLink.Common.csproj

# 3. Check results
cat build-results.json | jq '.success'
# Output: true

# 4. Update tracker
# Edit REMEDIATION-TRACKER.md to mark task complete
```

## Baseline Management

### Creating Baseline
Before starting remediation work:

```bash
# Build all projects and capture baseline
./scripts/create-build-baseline.sh

# Generates: build-baseline.json with warning counts per project
```

### Comparing to Baseline
After applying fixes:

```bash
# Compare current build to baseline
./scripts/compare-to-baseline.sh <project-path>

# Output: Shows new/removed warnings
```

## Troubleshooting

### Build Script Fails

**Symptom**: Script exits with error before build runs

**Solutions**:
- Verify .NET SDK installed: `dotnet --version`
- Check project path is correct
- Ensure execute permissions: `chmod +x scripts/validate-build.sh`

### Build Succeeds Locally, Fails in Script

**Symptom**: `dotnet build` works manually but script fails

**Solutions**:
- Check working directory in script
- Verify environment variables
- Ensure NuGet packages restored: `dotnet restore`

### Output Parsing Fails

**Symptom**: build-results.json is malformed

**Solutions**:
- Check for special characters in build output
- Verify jq is installed for JSON parsing
- Review stderr for unexpected messages

## Advanced Usage

### Multi-Project Validation

```bash
# Validate all projects in solution
./scripts/validate-solution.sh CadLink.sln

# Generates: build-results-all.json with per-project results
```

### Integration with Git Hooks

```bash
# Add to .git/hooks/pre-commit
#!/bin/bash
./scripts/validate-build.sh <changed-projects> || exit 1
```

### CI/CD Integration

```yaml
# Example Azure DevOps pipeline step
- script: |
    ./scripts/validate-build.sh $(ProjectPath)
    cat build-results.json
  displayName: 'Validate Build'
```

## Protocol Compliance

### For AI Assistants

When applying code fixes:

1. **DO NOT** run `dotnet build` directly via Bash tool
2. **DO** document that build validation is required
3. **DO** note the specific project that needs validation
4. **DO** update REMEDIATION-TRACKER.md with validation status

### For Developers

When completing remediation tasks:

1. **MUST** run validation script after code changes
2. **MUST** review build-results.json before marking complete
3. **SHOULD** compare to baseline for warning changes
4. **SHOULD** document any new warnings in tracker

## Metrics to Track

### Build Health Metrics
- **Success Rate**: % of builds that succeed
- **Warning Trend**: Change in warning count over time
- **Build Duration**: Average time to build per project
- **Error Types**: Distribution of error codes

### Remediation Metrics
- **Tasks Validated**: % of completed tasks with build validation
- **Fixes Per Build**: Average fixes before clean build
- **Regression Rate**: % of fixes that introduce new issues

## Related Documents

- **[REMEDIATION-TRACKER.md](../docs/code-review/REMEDIATION-TRACKER.md)** - Task tracking for P0-P3 fixes
- **[BUILD-METRICS-PROTOCOL.md](BUILD-METRICS-PROTOCOL.md)** - Build performance analysis
- **[CODE-REVIEW-PROTOCOL.md](CODE-REVIEW-PROTOCOL.md)** - Code review methodology
- **[TODO.md](../docs/code-review/TODO.md)** - Code review progress tracking

## Future Enhancements

### Planned Features
- Automated warning categorization
- Integration with static analysis tools
- Build time regression detection
- Parallel project builds
- Real-time build notifications

### Under Consideration
- Visual Studio solution analysis
- MSBuild log parsing
- Code coverage integration
- Performance profiling hooks

---

**Last Updated**: 2025-10-16
**Version**: 1.0
**Status**: Active

**Change Log**:
- 2025-10-16: Initial version
