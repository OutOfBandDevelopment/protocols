# Technical Research Protocol

For comprehensive research into standards, RFCs, specifications, and technology ecosystems.

## When to Use This Protocol

Use this protocol when:
- Researching technical standards (RFCs, IEEE, CNCF, ISO)
- Exploring technology ecosystems (alternatives, comparisons)
- Building comprehensive knowledge bases
- Documenting research findings with deep context
- Creating decision matrices for technology selection
- Gathering resources (implementations, libraries, tools)

Not for:
- Simple how-to guides
- Single-feature documentation
- Quick reference cards
- Basic tutorials

## Research Scope Definition

### 1. Topic Identification

Clarify what is being researched:
```
What is the problem domain?
  → Event streaming, blob storage, synchronization, etc.

What are the key constraints?
  → Scale, latency, reliability, cost, vendor lock-in, etc.

What is the desired outcome?
  → Decision matrix, comparison, implementation guide, ecosystem map
```

### 2. Stakeholder Perspective

Identify research from these viewpoints:
- **Requirements:** What's technically needed?
- **Economics:** Cost/benefit, open-source vs. commercial
- **Governance:** Licensing, standards bodies, FOSS philosophy
- **Operations:** Deployment patterns, complexity, support

## Research Dimensions

Research should cover these aspects:

### A. Standards & Specifications (Top Priority)

For each relevant standard:
- **RFC/Standard Number** - e.g., RFC 7826
- **Organization** - IETF, IEEE, CNCF, ISO, etc.
- **Status** - Draft, Proposed, Standard, Deprecated
- **Publication Date** - When published
- **Key Concepts** - Main innovations in standard
- **Use Cases** - When/why to use this standard
- **Limitations** - What it doesn't cover

**Download:** Primary standard documents to project `/documents/` folder (PDF)

### B. Open Standards vs. Proprietary

Mark clearly:
- ✅ **Open Standards** (published, independent implementations possible)
- ❌ **Proprietary** (vendor-controlled, no external spec)
- 🔄 **De Facto Standards** (widely adopted, but not formal standard)

### C. FOSS vs. Commercial

Create two tiers of recommendations:
1. **Primary:** FOSS + Open Standards
2. **Alternative:** Commercial/platform-specific (with reasons)

Document for each option:
- **License** - Exact license (Apache 2.0, BSD, AGPL, etc.)
- **Source** - GitHub link or home page
- **Open Standard?** - Yes/No, which standard if yes
- **Vendor Lock-in?** - Risk level
- **Cost Model** - FOSS, freemium, commercial
- **Support** - Community, commercial, enterprise

### D. Technology Comparison Matrix

Create for major options:
```
| Aspect | Tech A | Tech B | Tech C |
|--------|--------|--------|--------|
| Latency | X ms | Y ms | Z μs |
| Throughput | A msgs/sec | B msgs/sec | C msgs/sec |
| Operational | Simple | Complex | Expert |
| Cost | Free | $X/mo | $Y/mo |
| FOSS | ✅ | ❌ | ✅ |
```

### E. Implementation Options

For each major technology, document:
- **In-Process** - Libraries, patterns, constraints
- **Out-of-Process** - Services, deployment, scaling
- **Cloud** - Managed services, vendor-specific
- **Self-Hosted** - Private infrastructure

Example:
```
NATS:
  ├─ In-Process: NATS embedded client library
  ├─ Out-of-Process: NATS server (single binary)
  ├─ Cloud: Synadia managed, various cloud providers
  └─ Self-Hosted: Docker, Kubernetes, bare metal
```

### F. Ecosystem & Resource Index

For each technology:
- **Reference Implementations** - Official + community projects
- **Libraries** - By language (Python, Java, Go, C#, Rust, etc.)
- **Tools** - CLI, debugging, monitoring
- **Documentation** - Official, books, courses
- **Academic Papers** - Theory, benchmarks, comparisons
- **Real-World Use** - Case studies, production deployments

### G. Deployment Patterns

Document common patterns:
1. **Pattern Name** - Descriptive
2. **Architecture Diagram** - ASCII or PlantUML
3. **Components** - What's included
4. **Trade-offs** - Advantages and limitations
5. **When to Use** - Scenarios
6. **Example** - Concrete scenario

### H. Decision Tree / Quick Start Guide

Create visual decision-making tool:
```
START: What's your constraint?

├─ Latency < 1 microsecond?
│  └─ YES → LMAX Disruptor or Chronicle Queue
│  └─ NO → Continue
├─ Multi-region?
│  └─ YES → Kafka or Pulsar
│  └─ NO → Continue
└─ ... etc
```

### I. Research Bibliography

Organize references by type:

**Standards Documents (with download links):**
- RFCs with IETF Datatracker link
- IEEE standards with DOI
- ISO standards with reference
- CNCF specifications with GitHub link

**Academic Papers:**
- Author, title, year
- Publication venue (ACM, IEEE, arXiv, etc.)
- DOI or link
- Relevance summary (1-2 sentences)

**Books & Tutorials:**
- Title, author, publication year
- Online availability
- Relevance to topic

**Projects & Implementations:**
- Name, license, GitHub link
- Status (active, maintained, archived)
- Language/platform

### J. Lessons Learned & Anti-Patterns

Document:
- **What Works:** Validated approaches
- **What Doesn't:** Common mistakes, pitfalls
- **Gotchas:** Edge cases, surprising behaviors
- **Best Practices:** Proven patterns

### K. Glossary of Terms

Create comprehensive glossary covering:
- **Domain-Specific Terms** - Technical jargon specific to problem domain
- **Protocol/Standard Abbreviations** - RFC names, standard acronyms (RTSP, RTP, RTCP, etc.)
- **Product Names** - When referring to technologies (Kafka, NATS, Pulsar, etc.)
- **Architecture Patterns** - In-process, out-of-process, pub/sub, etc.
- **Performance Metrics** - Latency, throughput, cardinality, etc.
- **Standards Organizations** - IETF, IEEE, ISO, CNCF, etc.

**Format for Each Entry:**
- **Term** (exact capitalization as used in standards)
- Brief definition (1-2 sentences)
- Key context (RFC number if applicable, related terms)
- Common abbreviation if different from full name

**Organization:**
Group by category (Streaming & Protocols, Synchronization, Storage Formats, Event Streaming, Architecture, Networking, Standards, etc.) for quick reference.

## Documentation Structure

### Directory Layout - Standard Pattern

```
<research-project>/
├── README.md                     # Overview with TOC, research goals, status
├── INDEX.md                      # Navigation guide, quick reference with TOC
├── GLOSSARY.md                   # Glossary of domain-specific terms with TOC
│
├── <topic-subdirectories>/       # Organize by data type/topic (see guidance below)
│   ├── <TOPIC_1>.md              # Deep-dive document (e.g., event-streaming-standards.md)
│   └── <TOPIC_2>.md              # Deep-dive document
│
├── reference/                    # Navigation, reference, and meta documents
│   ├── RESEARCH_BIBLIOGRAPHY.md  # Standards, papers, implementations catalog
│   ├── FOSS-STANDARDS-FOCUS.md   # FOSS recommendations (if applicable)
│   └── PROJECT_COMPLETION_SUMMARY.md
│
├── documents/                   # Downloaded standards, RFCs (as PDF)
│   ├── RFC7826_RTSP2.0.pdf
│   ├── RFC8108_MultipleRTPStreams.pdf
│   └── ...
│
├── diagrams/                    # Architecture diagrams (PlantUML)
│   ├── messaging-architecture.puml
│   ├── decision-tree.puml
│   └── ...
│
└── references/                  # Research metadata
    └── downloaded-sources.md    # Registry of all sources
```

### Subdirectory Organization by Data Type

For larger research projects, organize document subdirectories by topic/data type:

**Example: Telemetry Streaming Project**
```
<research-project>/
├── streaming/       # Event streaming technologies & patterns
├── storage/         # Storage formats & databases
├── transfer/        # Reliable transfer protocols
├── protocols/       # Communication protocols
├── reference/       # Cross-cutting reference materials
└── diagrams/        # Architecture visualizations
```

**How to Determine Subdirectories:**
1. Identify primary data domains from research questions
2. Group related standards and technologies together
3. Create a subdirectory for each major category
4. Keep reference/ subdirectory for cross-cutting concerns (glossary, bibliography, navigation)
5. Use consistent naming: lowercase with hyphens

### File Naming Conventions

- **Topic docs:** `<subject>-<aspect>.md` (e.g., `event-streaming-standards.md`)
- **Downloaded RFCs:** `RFC<number>_<title>.pdf`
- **Diagrams:** `<architecture>-<variant>.puml`
- **Index/Navigation:** `INDEX.md`, `RESEARCH_BIBLIOGRAPHY.md`

## Document Templates

### Template 1: Standard/RFC Document

```markdown
# [RFC Number]: [Title]

**Status:** [Draft/Proposed/Standard/Deprecated]
**Published:** [Date]
**Organization:** [IETF/IEEE/CNCF/ISO]
**Link:** [Official URL]

## Overview
[2-3 sentences what it is]

## Table of Contents

- [Key Concepts](#key-concepts)
- [Strengths](#strengths)
- [Limitations](#limitations)
- [Use Cases](#use-cases)
- [Related Standards](#related-standards)

## Key Concepts
- Concept 1: Description
- Concept 2: Description

## Strengths
✅ Advantage 1
✅ Advantage 2

## Limitations
❌ Limitation 1
❌ Limitation 2

## Use Cases
- Scenario 1
- Scenario 2

## Related Standards
- RFC XXXX
- IEEE Standard
```

### Template 2: Technology Comparison

```markdown
## Technology: [Name]

**Status:** [FOSS/Commercial/Cloud]
**License:** [License type]
**Home:** [Official URL]

**Key Characteristics:**
- Throughput: X msgs/sec
- Latency: Y ms
- Operational: [Simple/Moderate/Complex]

## Table of Contents

- [Strengths](#strengths)
- [Limitations](#limitations)
- [When to Use](#when-to-use)
- [When NOT to Use](#when-not-to-use)
- [Comparison](#comparison)
- [Implementation](#implementation)

**Strengths:**
✅ Advantage
✅ Advantage

**Limitations:**
❌ Limitation
❌ Limitation

**When to Use:**
→ Scenario 1
→ Scenario 2

**When NOT to Use:**
❌ Scenario

**Comparison:**
[Table vs. alternatives]

## Implementation
- Language 1: Library X
- Language 2: Library Y
```

### Template 3: Pattern Document

```markdown
## Pattern: [Pattern Name]

**Use Case:** [What problem it solves]
**Complexity:** [Simple/Moderate/Expert]

## Table of Contents

- [Architecture](#architecture)
- [Components](#components)
- [Flow](#flow)
- [Advantages](#advantages)
- [Trade-offs](#trade-offs)
- [Real-World Example](#real-world-example)

**Architecture:**
[PlantUML diagram]

**Components:**
- Component 1: Role
- Component 2: Role

**Flow:**
1. Step 1
2. Step 2

**Advantages:**
✅ Advantage
✅ Advantage

**Trade-offs:**
⚠️ Trade-off
⚠️ Trade-off

**Real-World Example:**
[Concrete scenario]
```

## Research Process

### Phase 1: Scoping (1-2 hours)

- Define research question clearly
- Identify target audience
- Determine scope (breadth vs. depth)
- List expected deliverables

**Output:** Clear scope document

### Phase 2: Discovery (4-8 hours)

- Search for relevant RFCs/standards
- Identify key technologies
- Find academic papers
- Locate reference implementations

**Tools:**
- RFC Editor (rfc-editor.org)
- Google Scholar (scholar.google.com)
- GitHub search
- Standard organization websites (ISO, IEEE, CNCF, etc.)

**Output:** Annotated bibliography

### Phase 3: Analysis (8-16 hours)

- Read standards/papers
- Install/test technologies
- Create comparison matrices
- Document patterns and best practices

**Output:** Detailed analysis documents

### Phase 4: Synthesis (4-8 hours)

- Create decision trees
- Build recommendation hierarchy
- Write navigation guides
- Generate quick-start guides

**Output:** User-friendly reference materials

### Phase 5: Documentation (4-8 hours)

- Write comprehensive guides
- Create architecture diagrams
- Build cross-references
- Organize into coherent structure

**Output:** Complete research project

**Total Effort:** 20-40 hours for comprehensive research

## Quality Checklist

Before publishing research:

- [ ] All standards have links to official sources
- [ ] RFCs downloaded to `/documents/` folder
- [ ] FOSS vs. commercial clearly marked
- [ ] Open standards vs. proprietary noted
- [ ] Comparison matrices complete
- [ ] Decision tree created
- [ ] Real-world examples provided
- [ ] Cross-references to related projects added
- [ ] Bibliography complete with links
- [ ] Diagrams included (PlantUML preferred)
- [ ] Quick-start guides available
- [ ] Navigation/INDEX document created with TOC
- [ ] FOSS recommendations prioritized
- [ ] Licensing clearly documented
- [ ] Glossary created with TOC and Wikipedia references (where applicable)
- [ ] README includes project organization and TOC
- [ ] Documents organized into logical topic-based subdirectories
- [ ] Main documents include Tables of Contents for easy navigation
- [ ] Every main document includes TOC after executive summary (if applicable)
- [ ] All primary research documents follow document templates with TOC sections
- [ ] TOC links are working markdown anchors (verify with # headers)

## Distribution

### For Repositories

- Create project directory in `/projects/`
- Follow project protocol structure
- Cross-reference related projects
- Update CLAUDE.md with reference
- Link from project-ideas.md if applicable

### For Research Collections

- Create `/AnalogComputers/` style directory
- Maintain research-protocol.md for search strategies
- Keep download-sources.md registry updated
- Use consistent file naming

## Maintenance

### Periodic Updates (Quarterly)

- Check for new RFCs/standards
- Update technology status
- Refresh links
- Update benchmark data

### Version Control

- Tag releases (e.g., v1.0, v1.1)
- Document changes in CHANGELOG.md
- Maintain backwards compatibility in links

## Example Application

**Research Topic:** Event Streaming Alternatives to Kafka

**Scope:**
- RFCs/standards for event streaming
- FOSS technologies (primary)
- Commercial alternatives (secondary)
- Comparison for different use cases
- Implementation patterns

**Deliverables:**
1. RFC/standard catalog
2. Technology comparison matrix
3. FOSS vs. commercial recommendations
4. Decision tree
5. Deployment patterns
6. Quick-start guides
7. Bibliography with 100+ resources

**Structure:**
```
kafka-alternatives-research/
├── README.md (overview)
├── INDEX.md (navigation)
├── RESEARCH_BIBLIOGRAPHY.md (standards + resources)
├── event-streaming-standards.md (RFCs)
├── kafka-alternatives-comparison.md (technologies)
├── FOSS-STANDARDS-FOCUS.md (open source)
├── deployment-patterns.md (architectures)
├── documents/ (RFC PDFs)
├── diagrams/ (architecture diagrams)
└── references/
    └── downloaded-sources.md
```

---

*Protocol Version: 1.3*
*Last Updated: 2026-01-16*
*Created for: Synchronized Telemetry Streaming Research Project*

## Changelog

### v1.3 (2026-01-16) - TOC in Every Document
- Added requirement for TOC in every main document after executive summary
- Updated all three document templates to include TOC sections
- Added template guidance for TOC placement and structure
- Added quality checklist items for TOC verification:
  - TOC in every main document after executive summary
  - All primary documents follow templates with TOC
  - Working markdown anchor links for navigation
- Emphasized consistent TOC structure across project

### v1.2 (2026-01-16) - Subdirectory Organization
- Added explicit "Subdirectory Organization by Data Type" guidance
- Provided example: streaming/, storage/, transfer/, protocols/, reference/
- Added decision criteria for determining subdirectories
- Clarified that subdirectories should be organized by topic/data type
- Emphasized reference/ subdirectory for cross-cutting concerns
- Updated directory layout diagrams to show topic-based organization

### v1.1 (2026-01-16) - Glossary & Navigation
- Added Glossary of Terms dimension (K)
- Added glossary creation guidance with Wikipedia references
- Updated directory layout documentation to show GLOSSARY.md
- Added TOC requirements to quality checklist
- Added initial subdirectory organization guidance
- Emphasized Wikipedia references for domain-specific terms

### v1.0 (2026-01-16) - Initial Release
- Initial protocol specification
- 11 research dimensions (A-K)
- 5-phase research process
- Document templates and quality checklist
- Comprehensive quality assurance checklist

