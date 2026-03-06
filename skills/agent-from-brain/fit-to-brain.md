---
name: fit-brain-expert-to-brain
description: "Customize the generic brain-expert agent to a specific brain/ knowledge base by discovering the subject domain and conventions used in its files. Use when onboarding the brain-expert agent to a new subject repo."
user_invocable: true
---

# Fit Brain Expert to Brain

Take the generic brain-expert agent template and produce a subject-specific version by discovering what the `brain/` directory actually contains.

## Inputs

- The generic agent template at `agents/agent-from-brain.md`
- The `brain/` directory in the current repository

## Steps

### 1. Discover the brain directory

**Inventory the files:**
- List all files in `brain/` with Glob
- Read `brain/knowledge_gaps.md` if it exists

**Identify the subject domain:**
- What is the subject area? (infer from filenames and headings)
- Is there a central organizing concept or course the notes follow?

**Check file conventions:**
- What heading style do files use?
- What citation format is used? (APA, inline, footnotes, etc.)
- Are there any special character conventions? (e.g., IPA brackets, math notation)
- What does a typical file look like? Read 2–3 representative files.

**Check for a source textbook or course:**
- Do the citations reference a consistent source (e.g., a textbook, course number)?
- Is there a syllabus or reading list anywhere in the repo?

### 2. Write the subject-specific agent

Copy `agents/agent-from-brain.md` to `.claude/agents/{subject}-expert.md` (use a short, lowercase subject name, e.g., `linguistics-expert`, `biochem-expert`) and apply these changes:

**Frontmatter:**
- Update `name` to `{subject}-expert`
- Update `description` to reference the specific subject
- Update the examples to use realistic questions from this subject

**Opening line:**
- Replace the generic opening with the actual subject:
  > You are a study companion for {Subject} as documented in `brain/*.md`.

**Audience level (if applicable):**
- Replace "a student learning this subject" with the actual context (e.g., "an introductory linguistics student", "a first-year biochemistry student")

**File conventions (if different from generic):**
- Add any subject-specific character or notation conventions discovered in step 1
  - Example: "Do NOT use `<x>` angle brackets for orthographic forms — use `‹x›` (U+2039/U+203A)"
  - Example: "IPA transcriptions go in /slashes/ for phonemic and [brackets] for phonetic"
- Note the citation style actually used in the files

**Add a subject-specific context section** (before ## Tools):

```markdown
## Subject Context

{Subject} notes are organized around {organizing principle}. Key file areas include:

- `brain/{filename}.md` — {what it covers}
- `brain/{filename}.md` — {what it covers}
{repeat for notable files}

Primary source(s): {textbook, course, or author if identifiable}
```

### 3. Validate completeness

Verify the subject-specific agent:
- [ ] `name` reflects the actual subject
- [ ] Opening line names the subject explicitly
- [ ] Examples use realistic questions from this domain
- [ ] File conventions include any special notation used in the brain files
- [ ] Subject Context section lists the major brain files and their scope
- [ ] Primary source is identified if one is consistently cited
- [ ] No generic placeholder language remains (e.g., "the subject documented in")
- [ ] Output file is placed at `.claude/agents/{subject}-expert.md` in the repo
