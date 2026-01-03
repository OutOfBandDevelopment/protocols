# Development Protocols

Reusable protocols for software development workflows. These protocols are project-agnostic and can be used across any codebase.

---

## Quick Start

1. Run `init` to configure a new project
2. Use `start work on {id}` to begin a feature or bug fix
3. Follow the development workflow through review and testing

---

## Protocol Categories

| Category        | Path                       | Description                           |
|-----------------|----------------------------|---------------------------------------|
| Documentation   | `protocols/documentation/` | Standards for technical documentation |
| Software        | `protocols/software/`      | Architecture and code analysis        |
| Workflows       | `protocols/workflows/`     | Development lifecycle processes       |

---

## Workflow Protocols

| Trigger Phrase             | Protocol                | Purpose                    |
|----------------------------|-------------------------|----------------------------|
| `init`                     | init-setup.md           | Project initialization     |
| `start work on {id}`       | start-work.md           | Begin feature/bug work     |
| `review my changes`        | change-review.md        | Code review                |
| `create test plan`         | create-test-plan.md     | Test planning              |
| `show lifecycle`           | lifecycle.md            | View development workflow  |
| `pulled latest`            | review-branch-updates.md| Review branch updates      |

---

## Development Workflow

```
init                         <- First time project setup
  |
start work on {id}           <- Create branch, begin work
  |
[Implement changes]          <- Write code
  |
review my changes            <- Self-review before PR
  |
create test plan             <- Plan testing approach
  |
[Run tests]                  <- Execute test plan
  |
[Create pull request]        <- Submit for team review
```

---

## Documentation Protocols

| Protocol                    | Purpose                              |
|-----------------------------|--------------------------------------|
| documentation-style-guide   | Content standards for technical docs |
| documentation-standards     | File organization and naming         |
| documentation-structure     | Directory layout and templates       |

---

## Software Analysis Protocols

| Protocol                         | Purpose                          |
|----------------------------------|----------------------------------|
| architectural-analysis           | System architecture documentation|
| comprehensive-analysis-checklist | Documentation completeness       |
| security-audit                   | API security review              |
| compare-documentation-to-project | Gap analysis                     |
| protocol-validation              | Protocol quality assurance       |

---

## Usage

See [protocols/README.md](./protocols/README.md) for the complete protocol registry with detailed descriptions.

See [CLAUDE.md](./CLAUDE.md) for configuration rules and conventions.
