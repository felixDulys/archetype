---
name: fit-text-transcriber-to-source
description: "Customize the generic-text-transcriber agent to a specific document and repository by discovering the document's structure, format, and the project's concept file layout. Use when onboarding the transcriber agent to a new book, textbook, or manual in PDF, ODT, DOCX, or other document format."
user_invocable: true
---

# Fit Text Transcriber to Source

Take the generic text-transcriber agent template and produce a source-specific version by discovering the document's structure and the project's concept file organization.

## Inputs

- The generic agent template at `agents/generic-text-transcriber.md`
- A file path to the document (PDF, ODT, DOCX, plain text, etc.)
- The current repository's codebase

## Steps

### 1. Discover the source structure

Open and examine the document to answer each of these questions:

**Source identity:**
- What is the full title of the source?
- Who are the authors/editors?
- What is the publication year and edition?
- What is the file path?
- What is the file format?
- What citation format does the project use? (default to APA if unspecified)
- Write the full bibliographic reference in the chosen format.

**Document structure:**
- What is the extraction method? (Read tool for PDF, Python XML parsing for ODT/DOCX, pandoc, etc.)
- Test the extraction method on a sample section to confirm content is readable.
- How is the source organized? (parts → chapters → sections, flat chapters, modules, etc.)
- How many chapters/sections are there?
- For PDFs: what are the page ranges for each chapter/section?
- For ODT/DOCX: what are the heading markers for each chapter/section?
- What content types should be ignored? (images, exercises, quizzes — confirm defaults and note any source-specific additions)

**Extraction details:**
- Extract the table of contents or heading structure from the document.
- For PDFs: note the page numbers for each major section.
- For ODT/DOCX: note the heading text that marks each section boundary.
- Confirm that 2–3 sample sections can be successfully extracted and contain usable text.

### 2. Discover the project layout

Search the repo to answer:

**Unit-to-source mapping:**
- Is there a syllabus, config, or mapping file that links project units to source chapters? (e.g., `docs/syllabus.md`, `config/sources.yaml`)
- What are the project's units called? (modules, lessons, topics, weeks, etc.)
- Build the complete mapping table: `{project unit} → {source chapter(s)/section(s)}`

**Concept files:**
- Where do concept files live? (e.g., `brain/`, `concepts/`, `notes/`)
- What is the file naming convention? (e.g., `kebab-case.md`, `snake_case.md`)
- Which concept files already exist and which are empty?
- What concepts belong to each project unit?

**Citation conventions:**
- Does the project have an existing citation style or bibliography? Check for style guides, existing `## Sources` sections in concept files, or a central references file.
- If no convention exists, default to APA.

### 3. Write the source-specific agent

Copy `agents/generic-text-transcriber.md` to a new file (e.g., `agents/transcriber.md` or `agents/{source-name}-transcriber.md`) and apply these changes:

**Frontmatter:**
- Update `name` to reference the source (e.g., `algorithms-transcriber`)
- Update `description` to name the specific source

**Replace "Prerequisites — Discover Before Acting" with "Source Context":**
- Remove the discovery instructions — the agent no longer needs to figure these out at runtime
- Replace with a concrete section:

```markdown
## Source Context

**Source:** {full title} by {authors} ({year}).
**File:** `{file path}`
**Format:** {PDF/ODT/DOCX/etc.}

**Extraction method:** {specific method, e.g., "Read tool with pages parameter" for PDF, or the exact Python script for ODT/DOCX}

**Content to ignore:** {list of non-text content types for this source}
```

**Replace "Reading Document Files" with source-specific instructions:**
- Remove the generic multi-format section
- Replace with the exact extraction method for this document's format
- Include the tested extraction script or Read tool parameters

**Add a chapter/section reference:**

For PDFs:
```markdown
## Chapter Page Ranges

| Chapter | Title | Pages |
|---|---|---|
| 1 | {title} | 1–25 |
| 2 | {title} | 26–50 |
{...complete table}
```

For ODT/DOCX:
```markdown
## Chapter Markers

| Chapter | Heading text |
|---|---|
| 1 | {exact heading text} |
| 2 | {exact heading text} |
{...complete table}

Use these heading markers to locate chapter boundaries when extracting content.
```

**Add a unit-to-chapter mapping section:**

```markdown
## Unit Mapping

| {Unit type} | Source chapter(s) |
|---|---|
| {Unit 1} | Chapter {N} |
| {Unit 2} | Chapter {N} + Chapter {M} |
{...complete table}

When a unit spans multiple chapters, process one chapter at a time.
```

**Add a concept files section:**

```markdown
## Concept Files

Target directory: `{path}/`

| {Unit type} | Concept files |
|---|---|
| {Unit 1} | `{filename1}.md`, `{filename2}.md` |
{...complete table}
```

**Update Citation Rules with concrete values:**
- Replace generic citation instructions with the actual format and full reference
- Paste the exact inline citation form (e.g., `(Smith, 2023, Ch. 3, p. 45)`)
- Paste the exact `## Sources` block to use

**Update Workflow:**
- Remove "read the project's mapping file" — replace with direct references to the mapping table in the agent itself
- Step 1 becomes "look up the requested unit in the Unit Mapping table and identify the concept files from the Concept Files table"
- Step 2 becomes "look up the chapter page ranges/markers from the Chapter reference table"

### 4. Validate completeness

Verify the source-specific agent covers:
- [ ] Source title, authors, year, and file path are documented
- [ ] File format and extraction method are specified and tested
- [ ] Chapter/section reference (page ranges or heading markers) is present
- [ ] Complete unit-to-chapter mapping table is present
- [ ] Complete concept file listing per unit is present
- [ ] Full bibliographic reference is written out
- [ ] Inline citation format is specified with example
- [ ] Content-to-ignore list is tailored to the source
- [ ] No "discover", "locate", or "check for" language remains — everything is concrete
- [ ] No `[populate]` placeholders remain
