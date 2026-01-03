# Build Metrics Collection & Analysis Protocol

**Version**: 1.0
**Date**: 2025-10-15
**Purpose**: Collect build performance metrics when .NET SDK is available, then analyze with Claude Code

---

## Overview

This protocol defines a standardized approach for collecting and analyzing CadLink build metrics when the build environment is not directly accessible to Claude Code. The workflow is:

1. **User runs collection script** → Generates `build-metrics.json`
2. **User shares JSON file with Claude Code** → Uploads to chat
3. **Claude Code analyzes metrics** → Provides detailed performance report

---

## Phase 1: Collection (User Action Required)

### Prerequisites

- .NET SDK 8.0+ installed
- Visual Studio 2019/2022 (Windows) or .NET SDK (Linux/Mac)
- Access to CadLink source code

### Collection Scripts

Two scripts are provided for different platforms:

| Platform | Script | Location |
|----------|--------|----------|
| **Windows** | `build-metrics-collector.ps1` | `/current/src/scripts/` |
| **Linux/Mac** | `build-metrics-collector.sh` | `/current/src/scripts/` |

### Windows Instructions

```powershell
# Navigate to scripts directory
cd C:\Source\CadLink\scripts

# Option 1: Default settings (3 builds × Debug + Release)
.\build-metrics-collector.ps1

# Option 2: Custom settings
.\build-metrics-collector.ps1 `
    -SolutionPath "..\Source\CadLink.sln" `
    -OutputDir "..\build-metrics" `
    -BuildConfigurations "Debug,Release" `
    -Iterations 5

# Output will be saved to: ..\build-metrics\build-metrics.json
```

### Linux/Mac Instructions

```bash
# Navigate to scripts directory
cd /home/user/Source/CadLink/scripts

# Option 1: Default settings (3 builds × Debug + Release)
./build-metrics-collector.sh

# Option 2: Custom settings
./build-metrics-collector.sh \
    ../Source/CadLink.sln \
    ../build-metrics \
    "Debug,Release" \
    5

# Output will be saved to: ../build-metrics/build-metrics.json
```

### What Gets Collected

The script collects:

1. **System Information**
   - OS version, CPU cores, RAM
   - .NET SDK version
   - Build timestamp

2. **NuGet Restore Metrics**
   - Restore duration
   - Success/failure status

3. **Build Metrics** (per configuration per iteration)
   - Build duration (seconds)
   - Memory usage (MB)
   - Warning count
   - Error count
   - Per-project build times
   - Exit code

4. **Build Logs**
   - Text logs (`*.log`)
   - Binary logs (`*.binlog`) for MSBuild Structured Log Viewer

### Expected Output

```
build-metrics/
├── build-metrics.json          ← **Main file to share with Claude Code**
├── build-Debug-iter1.log       ← Detailed build log
├── build-Debug-iter1.binlog    ← Binary log (MSBuild viewer)
├── build-Debug-iter2.log
├── build-Debug-iter2.binlog
├── build-Debug-iter3.log
├── build-Debug-iter3.binlog
├── build-Release-iter1.log
├── build-Release-iter1.binlog
├── build-Release-iter2.log
├── build-Release-iter2.binlog
├── build-Release-iter3.log
└── build-Release-iter3.binlog
```

---

## Phase 2: Analysis (Claude Code Processes Results)

### Sharing with Claude Code

**Option 1: Direct File Upload**
```
1. Open Claude Code chat
2. Type: "I have build metrics to analyze"
3. Attach: build-metrics.json
4. Claude Code will automatically detect the protocol and analyze
```

**Option 2: Copy-Paste JSON**
```
1. Open Claude Code chat
2. Type: "Analyze these build metrics:"
3. Paste contents of build-metrics.json
4. Claude Code will parse and analyze
```

### Analysis Performed

When Claude Code receives `build-metrics.json`, it will:

1. **Validate Protocol**
   - Verify `Protocol: "CadLink-Build-Metrics-v1"`
   - Check JSON schema completeness

2. **Performance Analysis**
   - Calculate average, min, max, median build times
   - Compare Debug vs Release configurations
   - Identify build time variance (consistency check)
   - Analyze NuGet restore overhead

3. **Bottleneck Identification**
   - Identify slowest projects to build
   - Calculate per-project build time percentages
   - Recommend parallelization opportunities

4. **Warning/Error Analysis**
   - Categorize warnings by severity
   - Identify error patterns
   - Compare error rates across configurations

5. **System Performance Assessment**
   - CPU utilization efficiency
   - Memory usage patterns
   - Build scalability recommendations

6. **Comparison with Industry Benchmarks**
   - Compare against typical .NET solution build times
   - Assess if build performance is acceptable
   - Provide optimization recommendations

7. **Generate Remediation Report**
   - Specific recommendations for build time reduction
   - Cost-benefit analysis for build infrastructure upgrades
   - Estimated impact of parallelization improvements

### Example Analysis Output

Claude Code will generate a report like:

```markdown
# CadLink Build Performance Analysis

**Build Date**: 2025-10-15
**System**: Windows 10, 16 cores, 32 GB RAM
**Configurations Tested**: Debug, Release (3 iterations each)

## Summary

| Metric | Debug | Release | Delta |
|--------|-------|---------|-------|
| **Avg Build Time** | 245s | 287s | +17% |
| **Warnings** | 47 | 52 | +5 |
| **Errors** | 0 | 0 | - |

## Key Findings

1. **⚠️ Release builds slower than Debug** (+17%, 42 seconds)
   - Expected: Release should be 10-20% faster due to no symbol generation
   - Cause: Likely excessive optimization passes or code analysis
   - **Recommendation**: Review Release configuration settings

2. **✓ Build consistency excellent** (σ = 3.2s, 1.3% variance)
   - Minimal variation between iterations
   - Indicates stable build environment

3. **⚠️ Top 3 slowest projects** (48% of total build time)
   - CadLink.Common (45s, 18%)
   - CadLink.UI (38s, 15%)
   - Dashboard (36s, 14%)
   - **Recommendation**: Investigate dependency graphs

4. **NuGet restore overhead**: 23s (9% of total build time)
   - **Recommendation**: Configure local NuGet cache

## Detailed Recommendations

### Recommendation 1: Enable Binary Log Analysis
**Problem**: Cannot analyze MSBuild performance without binary logs
**Solution**: Open .binlog files in MSBuild Structured Log Viewer
**Tool**: https://msbuildlog.com/
**Expected Benefit**: Identify exact compilation bottlenecks

### Recommendation 2: Investigate CadLink.Common Dependencies
**Problem**: CadLink.Common takes 18% of build time, blocks 50+ projects
**Analysis Needed**:
- Check if project references can be reduced
- Consider splitting into smaller assemblies
- Review if full rebuild is triggered unnecessarily

### Recommendation 3: Optimize Release Configuration
**Problem**: Release builds are slower than Debug (unexpected)
**Solution**:
```xml
<!-- Check CadLink.sln for these settings: -->
<PropertyGroup Condition="'$(Configuration)' == 'Release'">
  <Optimize>true</Optimize>
  <DebugType>none</DebugType>  <!-- Or 'pdbonly' if debugging needed -->
  <DebugSymbols>false</DebugSymbols>
  <RunCodeAnalysis>false</RunCodeAnalysis>  <!-- Disable during build -->
</PropertyGroup>
```

## Cost-Benefit Analysis

**Current State**:
- Average build time: 266s (4.4 minutes)
- Daily builds per developer: ~20
- Daily time spent building: 88 minutes/developer
- Team size: 10 developers
- **Total daily build time**: 14.7 hours

**Target State** (with optimizations):
- Expected build time reduction: 30% (to 186s)
- Daily time saved: 27 minutes/developer
- Team time saved: 4.5 hours/day
- **Annual time saved**: 1,125 hours ($99,000 at $88/hour)

**Investment Required**:
- Binary log analysis: 4 hours ($352)
- Dependency graph optimization: 16 hours ($1,408)
- Build configuration tuning: 8 hours ($704)
- **Total**: 28 hours ($2,464)

**ROI**: $99,000 / $2,464 = **40x return**

## Next Steps

1. **Immediate** (Week 1, 4 hours):
   - Open .binlog files in MSBuild Structured Log Viewer
   - Identify exact bottlenecks in CadLink.Common build

2. **Short-Term** (Month 1, 16 hours):
   - Refactor CadLink.Common dependencies
   - Enable parallel project builds (if not already)
   - Configure local NuGet package cache

3. **Long-Term** (Month 2-3, 8 hours):
   - Implement incremental build verification
   - Set up build caching (Azure Artifacts or similar)
   - Monitor build performance over time

---

**Phase 36.1 Complete**: Build time analysis documented with actionable recommendations.
```

---

## JSON Schema Reference

### Protocol Identifier

```json
{
  "Version": "1.0",
  "Protocol": "CadLink-Build-Metrics-v1"
}
```

**Important**: Claude Code checks for `Protocol: "CadLink-Build-Metrics-v1"` to identify this data format.

### Full Schema

```json
{
  "Version": "1.0",
  "Protocol": "CadLink-Build-Metrics-v1",

  "SystemInfo": {
    "Timestamp": "2025-10-15T14:30:00Z",
    "Hostname": "DEVBOX01",
    "OS": "Windows 10",
    "OSVersion": "10.0.19045",
    "ProcessorCount": 16,
    "TotalMemoryGB": 32.0,
    "DotNetSDKVersion": "8.0.100",
    "MSBuildPath": "C:\\Program Files\\...",
    "SolutionPath": "C:\\Source\\CadLink\\CadLink.sln"
  },

  "RestoreMetrics": {
    "Success": true,
    "DurationSeconds": 23.45,
    "Timestamp": "2025-10-15T14:32:00Z"
  },

  "BuildMetrics": [
    {
      "Configuration": "Debug",
      "Iteration": 1,
      "Success": true,
      "ExitCode": 0,
      "DurationSeconds": 245.67,
      "MemoryUsedMB": 1024.5,
      "Warnings": 47,
      "Errors": 0,
      "LogFile": "../build-metrics/build-Debug-iter1.log",
      "BinLogFile": "../build-metrics/build-Debug-iter1.binlog",
      "ProjectBuildTimes": {
        "CadLink.Common": 45.2,
        "CadLink.UI": 38.1,
        "Dashboard": 36.4,
        "CadLink.Server": 28.9
      },
      "Timestamp": "2025-10-15T14:35:00Z"
    }
  ],

  "Summary": {
    "TotalBuilds": 6,
    "SuccessfulBuilds": 6,
    "FailedBuilds": 0,
    "AverageDurationSeconds": 266.3,
    "TotalWarnings": 294,
    "TotalErrors": 0
  }
}
```

### Field Descriptions

| Field | Type | Description |
|-------|------|-------------|
| `Version` | string | Protocol version (currently "1.0") |
| `Protocol` | string | **MUST** be "CadLink-Build-Metrics-v1" for Claude detection |
| `SystemInfo.Timestamp` | string | ISO 8601 UTC timestamp of collection start |
| `SystemInfo.Hostname` | string | Machine hostname |
| `SystemInfo.OS` | string | Operating system name |
| `SystemInfo.OSVersion` | string | OS version string |
| `SystemInfo.ProcessorCount` | number | CPU core count |
| `SystemInfo.TotalMemoryGB` | number | Total RAM in GB |
| `SystemInfo.DotNetSDKVersion` | string | .NET SDK version (from `dotnet --version`) |
| `SystemInfo.MSBuildPath` | string | (Windows only) Path to MSBuild.exe |
| `SystemInfo.SolutionPath` | string | Full path to CadLink.sln |
| `RestoreMetrics.Success` | boolean | NuGet restore success flag |
| `RestoreMetrics.DurationSeconds` | number | NuGet restore time in seconds |
| `RestoreMetrics.Timestamp` | string | ISO 8601 UTC timestamp |
| `BuildMetrics[].Configuration` | string | Build configuration (Debug/Release) |
| `BuildMetrics[].Iteration` | number | Iteration number (1-N) |
| `BuildMetrics[].Success` | boolean | Build success flag |
| `BuildMetrics[].ExitCode` | number | Build process exit code (0 = success) |
| `BuildMetrics[].DurationSeconds` | number | Build time in seconds |
| `BuildMetrics[].MemoryUsedMB` | number | (Windows only) Memory used in MB |
| `BuildMetrics[].Warnings` | number | Compiler warning count |
| `BuildMetrics[].Errors` | number | Compiler error count |
| `BuildMetrics[].LogFile` | string | Path to text build log |
| `BuildMetrics[].BinLogFile` | string | Path to binary log (.binlog) |
| `BuildMetrics[].ProjectBuildTimes` | object | (Optional) Per-project build times in seconds |
| `BuildMetrics[].Timestamp` | string | ISO 8601 UTC timestamp |
| `Summary.TotalBuilds` | number | Total number of builds performed |
| `Summary.SuccessfulBuilds` | number | Number of successful builds |
| `Summary.FailedBuilds` | number | Number of failed builds |
| `Summary.AverageDurationSeconds` | number | Average build time across all builds |
| `Summary.TotalWarnings` | number | Sum of all warnings |
| `Summary.TotalErrors` | number | Sum of all errors |

---

## Extending the Protocol

### Adding Custom Metrics

To add custom metrics, extend the JSON with additional top-level keys:

```json
{
  "Version": "1.0",
  "Protocol": "CadLink-Build-Metrics-v1",

  "CustomMetrics": {
    "DiskIOMetrics": { ... },
    "NetworkMetrics": { ... }
  }
}
```

Claude Code will process standard fields and optionally analyze custom metrics if recognized.

### Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-10-15 | Initial protocol definition |

---

## Troubleshooting

### Script Errors

**Problem**: "dotnet: command not found"
**Solution**: Install .NET SDK from https://dotnet.microsoft.com/download

**Problem**: "MSBuild not found" (Windows)
**Solution**: Install Visual Studio 2019/2022 with .NET desktop development workload

**Problem**: "Solution not found"
**Solution**: Verify `SolutionPath` parameter points to correct location

### Analysis Errors

**Problem**: Claude Code doesn't recognize the JSON
**Solution**: Verify `"Protocol": "CadLink-Build-Metrics-v1"` is present

**Problem**: Claude Code asks for more context
**Solution**: Share both `build-metrics.json` AND one `.binlog` file for deep analysis

---

## Phase 36 Completion

Once build metrics are collected and analyzed, **Phase 36.1: Build Time Analysis** will be marked complete in `TODO.md`:

```markdown
### Phase 36: Developer Experience & Build Performance ✅ COMPLETE
**Status**: Complete (Build metrics collected and analyzed)
**Completed**: 2025-10-15

- [x] **36.1 Build time analysis** - Measure clean build and incremental build times
  - File: `36-developer-experience-build-performance.md` ✅
  - Metrics: build-metrics.json collected and analyzed ✅
```

---

## Additional Tools

### MSBuild Structured Log Viewer

For deep-dive build analysis, use MSBuild Structured Log Viewer:

**Website**: https://msbuildlog.com/

**Usage**:
1. Download MSBuildStructuredLogViewer from https://msbuildlog.com/
2. Open `build-Debug-iter1.binlog`
3. Analyze:
   - Task execution timeline
   - Project dependency graph
   - File I/O operations
   - CPU/memory usage

**Benefits**:
- Visual timeline of build process
- Identify exact bottlenecks
- See which projects block parallelization
- Analyze incremental build behavior

---

## Success Criteria

Phase 36.1 is considered **COMPLETE** when:

1. ✅ Build metrics collected successfully (6+ builds)
2. ✅ JSON file generated and validated
3. ✅ Claude Code analyzes metrics and provides report
4. ✅ At least 3 actionable recommendations identified
5. ✅ TODO.md updated with completion status

---

## Related Documentation

- **Phase 36 Document**: `/current/src/docs/code-review/36-developer-experience-build-performance.md`
- **TODO Tracking**: `/current/src/docs/code-review/TODO.md`
- **Collection Scripts**: `/current/src/scripts/build-metrics-collector.*`

---

**Document Version**: 1.0
**Last Updated**: 2025-10-15
**Protocol Status**: Active
