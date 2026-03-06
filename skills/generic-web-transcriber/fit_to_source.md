---
name: fit-transcriber-to-source
description: "Customize the generic-web-transcriber agent to a specific source and repository by discovering the source's structure, URL patterns, and the project's concept file layout. Use when onboarding the transcriber agent to a new book, textbook, or documentation site."
user_invocable: true
---

# Fit Web Transcriber to Source

Take the generic web-transcriber agent template and produce a source-specific version by discovering the web source's structure and the project's concept file organization.

## Inputs

- The generic agent template at `agents/generic-web-transcriber.md`
- A URL or name of the web-published source (book, textbook, documentation)
- The current repository's codebase

## Steps

### 1. Discover the source structure

Fetch the source's landing page or table of contents using `WebFetch` and answer each of these questions:

**Source identity:**
- What is the full title of the source?
- Who are the authors/editors?
- What is the publication year and edition?
- What is the base URL?
- What citation format does the project use? (default to APA if unspecified)
- Write the full bibliographic reference in the chosen format.

**URL patterns:**
- What is the URL pattern for chapter/part intro pages? (e.g., `{base}/part/chapter-{N}-{slug}/`)
- What is the URL pattern for individual sections? (e.g., `{base}/chapter/{N}-{M}-{slug}/`)
- Are there other URL tiers (sub-sections, appendices, glossary)?
- Fetch 2–3 sample pages to confirm the patterns work and that content is extractable via `WebFetch`.

**Content structure:**
- How is the source organized? (parts → chapters → sections, flat chapters, modules, etc.)
- How many chapters/sections are there?
- What content types should be ignored? (videos, quizzes, interactive elements — confirm defaults and note any source-specific additions like embedded iframes, code playgrounds, etc.)

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

Copy `agents/generic-transcriber.md` to a new file (e.g., `agents/transcriber.md` or `agents/{source-name}-transcriber.md`) and apply these changes:

**Frontmatter:**
- Update `name` to reference the source (e.g., `linguistics-transcriber`)
- Update `description` to name the specific source

**Replace "Prerequisites — Discover Before Acting" with "Source Context":**
- Remove the discovery instructions — the agent no longer needs to figure these out at runtime
- Replace with a concrete section:

```markdown
## Source Context

**Source:** {full title} by {authors} ({year}). {URL}

**URL Patterns:**
- Chapter intro: `{pattern with actual base URL}`
- Section: `{pattern with actual base URL}`

**Content to ignore:** {list of non-text content types for this source}
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
- Paste the exact inline citation form (e.g., `(Anderson et al., 2022, Section X.X)`)
- Paste the exact `## Sources` block to use

**Update Workflow:**
- Remove "read the project's mapping file" — replace with direct references to the mapping table in the agent itself
- Step 1 becomes "look up the requested unit in the Unit Mapping table and identify the concept files from the Concept Files table"

### 4. Validate completeness

Verify the source-specific agent covers:
- [ ] Source title, authors, year, and URL are documented
- [ ] URL patterns are tested and confirmed working (fetched sample pages successfully)
- [ ] Complete unit-to-chapter mapping table is present
- [ ] Complete concept file listing per unit is present
- [ ] Full bibliographic reference is written out
- [ ] Inline citation format is specified with example
- [ ] Content-to-ignore list is tailored to the source
- [ ] No "discover", "locate", or "check for" language remains — everything is concrete
- [ ] No `[populate]` placeholders remain
