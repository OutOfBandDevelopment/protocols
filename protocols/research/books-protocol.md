# Books Protocol

**Purpose:** Maintain `books.md` - the inventory of technical books for software design, hardware, electronics, and computer architecture.

## Overview

`books.md` is a centralized inventory of all technical books owned, providing a single source of truth for book references used across the repository.

**Key Principle:** Books are documented separately from tools and consumables. Books that inform projects, patterns, and technical work deserve dedicated tracking.

## When to Add Books

Add books to `books.md` when:
1. **Technical reference books** - Software, hardware, electronics, programming
2. **Architecture and design** - Software patterns, system design
3. **Computer science** - Algorithms, data structures, theory
4. **Engineering** - Digital logic, analog computing, FPGA, embedded systems
5. **Already owned** - Even if purchase info unknown, document ownership

## When NOT to Add Books

Do NOT add to books.md:
1. **Fiction/non-technical** - Novels, general interest
2. **Cookbooks** - Unless electronics-themed (e.g., "555 Timer IC Circuits Cookbook")
3. **Magazines/periodicals** - One-time publications
4. **Manuals** - Equipment manuals documented with equipment
5. **Datasheets** - Component datasheets documented with equipment

## File Structure

```markdown
# Technical Books & References

Personal library of technical books.

---

## Software Design & Architecture

| Title | Author | Topic | Publisher | Purchased | Price |
|-------|--------|-------|-----------|-----------|-------|
| Design Patterns: Elements of Reusable Object-Oriented Software | Gang of Four | Design patterns (GoF) | Addison-Wesley | TBD | TBD |

*Referenced in [diagrams-and-patterns/](diagrams-and-patterns/) - Software architecture patterns library*

---

## Hardware & Electronics

| Title | Author/Publisher | Topic | Purchased | Price |
|-------|------------------|-------|-----------|-------|
| Digital Computer Electronics | Paul Malvino | Computer architecture, SAP-1 | 2020-01-09 | $29.44 |

---

## Online Resources

### Software Design
- [Wikipedia link](url) - Description

### Hardware & Electronics
- [Resource link](url) - Description

---

## Cross-References

### Related Documentation
- Links to equipment, projects, etc.

---

*Last updated: YYYY-MM-DD*
```

## Section Organization

### 1. Software Design & Architecture

Books covering:
- Design patterns (GoF, CQRS, etc.)
- Software architecture (SOA, microservices, event-driven)
- Code quality and craftsmanship
- Modeling (UML, use cases)
- Language-specific patterns (.NET, etc.)

**Format:**
```markdown
| Title | Author | Topic | Publisher | Purchased | Price |
```

### 2. Hardware & Electronics

Books covering:
- Digital logic and computer architecture
- FPGA/VHDL/Verilog programming
- Analog computing
- Op-amps and electronics
- Embedded systems
- Microcontroller programming

**Format:**
```markdown
| Title | Author/Publisher | Topic | Purchased | Price |
```

### 3. Online Resources

Web-based references organized by category:
- Documentation sites
- Wikipedia articles
- Specification documents (PDFs)
- Online tutorials/courses

### 4. Cross-References

Links to:
- Projects that use these books
- Equipment documentation
- Other related inventories

## Entry Format

### Software Design & Architecture Books

```markdown
| Title | Author | Topic | Publisher | Purchased | Price |
|-------|--------|-------|-----------|-----------|-------|
| Design Patterns: Elements of Reusable Object-Oriented Software | Gang of Four (Gamma, Helm, Johnson, Vlissides) | Design patterns (GoF) | Addison-Wesley | TBD | TBD |
```

**Columns:**
- **Title**: Full book title (capitalize properly)
- **Author**: Full author name(s) or team name
- **Topic**: Brief subject description (1-5 words)
- **Publisher**: Publishing company
- **Purchased**: Date in YYYY-MM-DD format, or "TBD" if unknown
- **Price**: Purchase price, or "TBD" if unknown

### Hardware & Electronics Books

```markdown
| Title | Author/Publisher | Topic | Purchased | Price |
|-------|------------------|-------|-----------|-------|
| Digital Computer Electronics | Paul Malvino | Computer architecture, SAP-1, digital logic | 2020-01-09 | $29.44 |
```

**Columns:**
- **Title**: Full book title
- **Author/Publisher**: Author if known, otherwise publisher/vendor
- **Topic**: Brief description of subject matter
- **Purchased**: Date in YYYY-MM-DD format
- **Price**: Purchase price paid

### Books with Unknown Purchase Info

For books already owned but purchase info unknown:
```markdown
| Building Microservices | Sam Newman | Microservices architecture | O'Reilly | TBD | TBD |
```

This documents ownership even without purchase records.

## Adding New Books

### Step-by-Step Process

1. **Determine category**: Software or Hardware?
2. **Find correct subsection** in books.md
3. **Add new entry** with all fields
   - Use "TBD" for unknown dates/prices
   - Include full author names
   - Be specific in Topic field
4. **Sort by relevance** (most important/referenced first)
5. **Update cross-references** if book is referenced elsewhere
6. **Update last-modified date** at bottom of file

### Example: Adding a New Software Book

```markdown
## Software Design & Architecture

| Title | Author | Topic | Publisher | Purchased | Price |
|-------|--------|-------|-----------|-----------|-------|
| Design Patterns: Elements of Reusable Object-Oriented Software | Gang of Four | Design patterns (GoF) | Addison-Wesley | TBD | TBD |
| Clean Architecture | Robert C. Martin | Software architecture | Prentice Hall | 2024-03-15 | $32.99 |  <!-- NEW -->
| The Pragmatic Programmer | Andrew Hunt, David Thomas | Software craftsmanship | Addison-Wesley | TBD | TBD |
```

## Cross-References

### From Other Files to books.md

**In diagrams-and-patterns/README.md:**
```markdown
### Books
See [books.md](../books.md#software-design--architecture) for complete book inventory including:
- Design Patterns: Elements of Reusable Object-Oriented Software (Gang of Four)
- The Pragmatic Programmer
- [List 5-8 key titles]
```

**In project READMEs:**
```markdown
## References

Books used:
- [Digital Computer Electronics](../books.md#hardware--electronics) by Malvino - SAP-1 architecture
- [Verilog by Example](../books.md#hardware--electronics) - HDL reference
```

**In tools-and-components.md:**
```markdown
*For technical books, see [books.md](books.md)*
```

### From books.md to Other Files

**In books.md cross-references section:**
```markdown
## Cross-References

### Related Documentation
- [diagrams-and-patterns/](diagrams-and-patterns/) - Software architecture patterns library (references software design books)

### Projects Using These References
- [SAP-1 Computer](projects/sap-1-computer/) - Uses *Digital Computer Electronics* by Malvino
- [FPGA CPU Design](projects/fpga-cpu-design/) - References Verilog/VHDL books
```

## Online Resources Section

### Adding Web References

```markdown
## Online Resources

### Software Design
- [Software design patterns](https://en.wikipedia.org/wiki/Software_design_pattern) - Wikipedia overview
- [Object Constraint Language (OCL)](https://www.omg.org/spec/OCL/2.4/PDF) - OMG specification

### Hardware & Electronics
- [Analog Computers Repository](AnalogComputers/analog-computing-repository/) - 232 PDFs covering fundamentals
```

**Guidelines:**
- Organize by category (Software Design, Hardware & Electronics, etc.)
- Include brief description after each link
- Link to online specs, PDFs, Wikipedia articles
- Link to local repositories (AnalogComputers/, etc.)

## Maintenance

### Regular Updates

**When to update:**
1. After purchasing new book - add immediately
2. When starting a project that uses a book - add cross-reference
3. When finding purchase info for "TBD" books - fill in details
4. Quarterly review - ensure all owned books documented

### Sorting Within Sections

**Preferred order:**
1. Most referenced books first (Design Patterns, Pragmatic Programmer)
2. Core textbooks (Computer Architecture, Digital Computer Electronics)
3. Specialized books (CQRS, specific technologies)
4. Newest additions last

**Don't obsess over perfect sorting** - keep frequently referenced books near top.

## Common Mistakes to Avoid

❌ **Don't:**
- Add non-technical books (fiction, general interest)
- Forget to fill in "TBD" when you know the info
- Mix up Software vs Hardware sections
- Add equipment manuals (those go with equipment docs)
- Add magazine subscriptions or periodicals
- Use shorthand titles ("GoF book" instead of full title)

✅ **Do:**
- Use full, official book titles
- Include subtitle if it adds clarity
- Expand author names (not just initials)
- Use "TBD" for unknown purchase info
- Link to projects that use the book
- Keep cross-references updated

## Examples

### Good Entries

```markdown
| Design Patterns: Elements of Reusable Object-Oriented Software | Gang of Four (Gamma, Helm, Johnson, Vlissides) | Design patterns (GoF) | Addison-Wesley | TBD | TBD |
| The Zynq Book | Louise H. Crockett et al. | Embedded ARM Cortex-A9, Zynq SoC | Strathclyde Academic Media | 2021-12-30 | $27.49 |
```

### Bad Entries

```markdown
| GoF Book | GoF | Patterns | ? | ? | ? |  <!-- Bad: unclear title, missing info -->
| Arduino manual | Arduino.cc | Arduino | N/A | Free | N/A |  <!-- Bad: manual, not a book -->
| The Hobbit | Tolkien | Fantasy | HarperCollins | 2020-01-01 | $15.99 |  <!-- Bad: not technical -->
```

## Integration with Other Inventories

### Books vs Equipment Manuals

**Books (books.md):**
- Published technical books
- ISBN numbers
- Purchased/owned
- General reference

**Equipment Manuals (with equipment):**
- Vendor-provided documentation
- Specific to owned equipment
- Often PDFs downloaded from vendor
- Equipment-specific

### Books vs Papers/PDFs

**Books (books.md):**
- Complete published books
- ISBN numbers
- Physical or ebook

**Research Papers (AnalogComputers/, projects/):**
- Academic papers
- Application notes
- Technical reports
- Organized by topic/project

## Protocol Updates

When to update this protocol:
- Adding new book categories
- Changing cross-reference patterns
- New related inventories created
- User feedback on organization

---

*Protocol Version: 1.0*
*Created: 2026-01-07*
