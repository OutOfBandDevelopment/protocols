# Claude Code Configuration for CadLink

This file contains important information and protocols for working with the CadLink codebase.

## Project Overview

**Name:** CadLink
**Type:** Enterprise Medical Data Management System
**Languages:** C# (.NET Framework 4.8), WiX (Windows Installer)
**Repository:** Git
**CI/CD:** Azure DevOps
**Work Items:** Azure DevOps Boards

## Startup Instructions for Claude

**IMPORTANT:** At the start of each new interaction, perform the following auto-discovery steps:

1. **Check for Available Protocols:**
   - Use the Glob tool to search for all protocol files: `.claude/protocols/*.md`
   - List all protocols found

2. **Load Each Protocol:**
   - Read each protocol file to understand its purpose
   - Identify trigger phrases for each protocol
   - Note what the protocol does and when to use it

3. **Protocol Registry:**
   - Keep a mental registry of all available protocols
   - Be ready to apply them when trigger phrases are detected
   - Proactively suggest protocols when relevant to user requests

4. **Example Discovery Process:**
   ```
   Found protocols:
   - .claude/protocols/change_review.md
     Triggers: "review my changes", "code review", "review this change"
     Purpose: Comprehensive code review with Azure DevOps integration

   - .claude/protocols/[future_protocol].md
     Triggers: [to be discovered]
     Purpose: [to be discovered]
   ```

**Why This Matters:**
- New protocols can be added without modifying CLAUDE.md
- Ensures all protocols are available and ready to use
- Prevents missed opportunities to apply helpful protocols
- Keeps protocol knowledge current and comprehensive

## Directory Structure

```
CadLink/
├── Source/
│   ├── CadLink.Server/          - Server application
│   ├── CadLink.Client/          - Client application
│   ├── CadLink.Common/          - Shared libraries
│   ├── Tools/
│   │   ├── DbKeyCustomAction/   - Installer custom actions
│   │   └── Installers/          - WiX installer projects
│   └── ...
├── InternalTools/
│   └── EncryptionManager/       - Database encryption tools
│       └── KeyDatabaseLegacy/   - Legacy encryption CLI tool
├── Tests/
│   ├── CadLink.Tests/           - Unit tests
│   └── CadLinkAutomatedTests/   - Integration tests
├── docs/
│   ├── notes/                   - Feature documentation
│   └── testing/                 - Test plans
├── .claude/
│   ├── protocols/               - Code review and other protocols
│   └── CLAUDE.md               - This file
└── README.md
```

## Protocols

### Development Lifecycle

**See:** `.claude/protocols/lifecycle.md` for complete workflow documentation

**Quick Reference:**
```
1. start work on {work item}  → Create branch and docs
2. [Implement changes]         → Write code
3. review my changes           → Comprehensive review
4. create test plan            → Generate test plan
5. [Implement & run tests]     → Execute tests
6. [Create pull request]       → Submit for review
```

### Available Protocols

| Protocol | Trigger Phrases | Prerequisites | Purpose |
|----------|----------------|---------------|---------|
| **Lifecycle** | "show lifecycle", "show workflow" | None | Complete development workflow documentation |
| **Start Work** | "start work on {number}" | None | Create branch, foundation docs, implementation plan |
| **Create Docs** | "create documentation" | None | Create feature documentation structure |
| **Code Review** | "review my changes" | Docs must exist | Comprehensive review, updates docs with summary |
| **Test Plan** | "create test plan" | Review complete | Generate test plan, update docs with link |

**For detailed protocol steps, see:** `.claude/protocols/`

## Documentation Standards

### Code Documentation Location: `docs/notes/`

**When to Create Documentation:**
- New features
- Security fixes
- Breaking changes
- Complex refactorings
- Multi-component changes

**File Naming Conventions:**
- `{Component}-{Feature}.md` - e.g., `KeyDatabaseLegacy-PasswordSecurityFix.md`
- `{WorkItem}-{ShortDescription}.md` - e.g., `115163-PlaintextPasswordFix.md`
- `{Feature}-Implementation.md` - e.g., `BatchSizeAutoScaling-Implementation.md`

**Required Sections:**
1. Overview - What and why
2. Branch Information - Branch name, commits, work item
3. Problem Statement - What was broken/missing
4. Solution - How it was fixed
5. Changes Made - All files with before/after code
6. Impact Analysis - Components affected, breaking changes
7. Testing - How to test, scenarios covered
8. Deployment - Build requirements, deployment steps
9. Use Cases/Examples - Real-world scenarios
10. Related Documentation - Links to other docs
11. Troubleshooting - Common issues

### Existing Documentation

**Feature Documentation:**
- `docs/notes/KeyDatabaseLegacy-PasswordSecurityFix.md` - Password security enhancement (Work Item #115163)
- `docs/notes/CreateEncryptedConfiguration.md` - Encrypted configuration creation
- `docs/notes/UploadProgressActivity.md` - Upload progress tracking

**Component README Files:**
- `InternalTools/EncryptionManager/KeyDatabaseLegacy/README.md` - KeyDatabaseLegacy user guide
- `InternalTools/EncryptionManager/KeyDatabaseLegacy/README_TEST.md` - Testing guide

**Test Plans:**
Test plans are stored in `docs/testing/` and match the naming of their corresponding feature documentation in `docs/notes/`.

**Naming Convention:**
- Match feature doc name: `docs/notes/KeyDatabaseLegacy-PasswordSecurityFix.md`
- Test plan location: `docs/testing/KeyDatabaseLegacy-PasswordSecurityFix.md`

**When to Create Test Plans:**
- After running change review protocol
- Use "create test plan" protocol to generate comprehensive test documentation
- Include test scenarios, test cases, automated test specs, and manual procedures

## Branch Naming Conventions

**Format:**
- `features/{workitem}-{description}` - New features
- `bugs/{workitem}-{description}` - Bug fixes
- `support/{version}` - Support/patch branches
- `users/{username}/{description}` - Personal/experimental branches

**Examples:**
- `features/115163-plaintext-password-fix`
- `bugs/104945-live-data-writer-rebuild`
- `support/6.3.x`
- `users/mattw/claude-docs`

## Azure DevOps Integration

**Project:** CadLink

**Available MCP Tools:**
- `mcp__azure-devops__wit_get_work_item` - Get work item details
- `mcp__azure-devops__wit_list_work_item_comments` - Get work item comments
- `mcp__azure-devops__search_workitem` - Search for work items
- `mcp__azure-devops__repo_*` - Repository operations
- `mcp__azure-devops__pipelines_*` - Pipeline operations

**Work Item References:**
- Always include work item number in branch name
- Reference work items in commit messages with `#workitem`
- Extract work item context during code reviews

## Common Patterns

### Multi-Component Architecture

Many changes affect multiple components:
1. **Runtime** - `Source/CadLink.Server/`, `Source/CadLink.Client/`, `Source/CadLink.Common/`
2. **Installers** - `Source/Tools/Installers/` (Server32/64, Client32/64)
3. **Tools** - `InternalTools/EncryptionManager/`
4. **Tests** - `Tests/CadLink.Tests/`, `Tests/CadLinkAutomatedTests/`

**Critical:** When making a change in one component, always search for similar patterns in other components.

### Security Patterns

**Password/Secret Handling:**
- NEVER pass passwords via command-line arguments
- Use environment variables for sensitive data
- Clear environment variables after use
- Example: KeyDatabaseLegacy password security fix (Work Item #115163)

**Common Vulnerabilities to Check:**
- SQL Injection - Use parameterized queries
- XSS - Encode output
- Command Injection - Validate input, use ProcessStartInfo safely
- Path Traversal - Validate paths
- Sensitive Data Exposure - Passwords, keys, connection strings

### Database Encryption

**Legacy Encryption** (pre-5.3):
- Uses custom SQLite encryption
- Managed by KeyDatabaseLegacy.exe
- Being migrated to SEE encryption

**SEE Encryption** (5.3+):
- SQLite Encryption Extension
- Standard encryption method
- Handles database upgrades via installers

**Key Components:**
- `InternalTools/EncryptionManager/KeyDatabaseLegacy/` - CLI tool
- `Source/CadLink.Common/LegacySupport/` - Runtime support
- `Source/Tools/DbKeyCustomAction/` - Installer custom action
- `InternalTools/EncryptionManager/EncryptionManager/` - GUI tool

## Build Configuration

**Target Framework:** .NET Framework 4.8
**Platforms:** x86, x64
**Build Tools:** MSBuild, WiX Toolset
**Installer Technology:** WiX (Windows Installer XML)

**Key Build Files:**
- `*.sln` - Solution files
- `*.csproj` - Project files
- `Directory.Build.props` - Shared build properties
- `*.wixproj` - Installer projects

## Testing

**Unit Tests:** `Tests/CadLink.Tests/`
**Integration Tests:** `Tests/CadLinkAutomatedTests/`

**Test Naming:** Descriptive method names
**Test Coverage:** Expected for new features and bug fixes

## Git Workflow

**Main Branch:** `master`
**Protected Branches:** `master`, `support/*`

**Commit Message Format:**
```
Brief description of change

Detailed explanation if needed.

Work Item: #{number}
```

**Before Committing:**
1. Run code review protocol
2. Ensure all tests pass
3. Generate/update documentation
4. Verify no passwords or secrets in code

## Code Review Checklist

When the code review protocol runs, it checks:

✓ Branch name follows conventions
✓ Work item exists and is referenced
✓ All similar patterns updated across codebase
✓ Security vulnerabilities addressed
✓ Test coverage adequate
✓ Documentation generated/updated
✓ Build configuration updated
✓ Installer changes (if needed)
✓ No breaking changes (or documented)

## Special Considerations

### Installer Changes

When modifying code used by installers:
1. Update DbKeyCustomAction if database operations change
2. Rebuild all 4 installer projects (Server32/64, Client32/64)
3. Test upgrade scenarios from previous versions
4. Document in feature documentation

### Configuration Changes

When adding configuration:
1. Update `CadLinkServerSettings.xml` template
2. Update `CadLinkClientSettings.xml` template
3. Update ConfigurationSettings class
4. Document in user guides

### Breaking Changes

When making breaking changes:
1. Create migration guide in `docs/notes/`
2. Update version history
3. Document rollback procedure
4. Test upgrade paths

## Best Practices

1. **Search Before Implementing** - Use protocol to find similar patterns
2. **Security First** - Always consider security implications
3. **Document Everything** - Generate docs during code review
4. **Test Thoroughly** - Unit, integration, and manual tests
5. **Think Multi-Component** - Runtime, installer, tools, tests
6. **Link Work Items** - Always reference Azure DevOps work items
7. **Review Before Commit** - Run code review protocol

## Quick Commands

### Development Workflow
```
start work on {work item number}    # Phase 1: Start
create documentation                # Phase 1: Create docs (if needed)
review my changes                   # Phase 3: Review
create test plan                    # Phase 4: Test plan
show lifecycle                      # View complete workflow
```

**See:** `.claude/protocols/lifecycle.md` for detailed workflow

### Check Git Status
```bash
git status --short
git log --oneline -10
```

### Search for Patterns
```bash
grep -r "pattern" --include="*.cs"
find . -name "*Component*" -type f
```

### List Documentation
```bash
ls docs/notes/*.md
ls docs/testing/*.md
```

## Resources

- **Azure DevOps:** https://dev.azure.com/cadwell/CadLink
- **Protocols:** `.claude/protocols/`
  - Lifecycle: `lifecycle.md`
  - Start Work: `start_work.md`
  - Create Documentation: `create_documentation.md`
  - Code Review: `change_review.md`
  - Create Test Plan: `create_test_plan.md`
- **Documentation:**
  - Feature Documentation: `docs/notes/`
  - Test Plans: `docs/testing/`
  - Main README: `README.md`

## Version History

- **2026-01-02** - Created lifecycle.md protocol for complete development workflow
- **2026-01-02** - Simplified CLAUDE.md to reference protocols instead of duplicating content
- **2026-01-02** - Split documentation creation into separate create_documentation.md protocol
- **2026-01-02** - Updated change_review.md to require documentation exists first, only updates existing docs
- **2026-01-02** - Updated create_test_plan.md to update feature docs with test plan reference
- **2026-01-02** - Updated start_work.md to call create_documentation protocol
- **2026-01-02** - Established workflow: create docs → review → create test plan
- **2026-01-02** - Created start_work.md and create_test_plan.md protocols
- **2026-01-02** - Updated change_review.md to require Change Review Summary in documentation
- **2026-01-02** - Added auto-discovery instructions for protocols at interaction startup
- **2026-01-02** - Created CLAUDE.md and change_review.md protocol
- **2025-12** - Added KeyDatabaseLegacy password security documentation

---

**Last Updated:** 2026-01-02
**Maintainer:** Matthew Whited
