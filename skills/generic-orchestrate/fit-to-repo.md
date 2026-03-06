---
name: fit-orchestrate-to-repo
description: Customize the generic orchestrate skill to a specific repository by discovering its package structure, available skills/agents, and workflow conventions. Use when onboarding the orchestrate skill to a new codebase.
user_invocable: true
---

# Fit Orchestrate to Repo

Take the generic orchestrate skill template and produce a repo-specific version by discovering the project's actual structure, tooling, and conventions.

## Inputs

- The generic skill template at `skills/generic-orchestrate/SKILL.md`
- The current repository's codebase and `.claude/` directory

## Steps

### 1. Discover the repo's Claude tooling

Search `.claude/` (or wherever skills/agents live) to answer:

**Skills:**
- What skills are available? (list names and one-line descriptions from each SKILL.md frontmatter)
- Which skill handles planning/scouting? (e.g. `scout-plan-execute`, `plan`, `architect`)
- Which skill handles code review or audit? (e.g. `pipeline-review`, `code-review`)
- Which skill handles design or frontend work?
- Are there any domain-specific skills relevant to sequencing work?

**Agents:**
- What subagents exist? (list names and purposes)
- Which agents are invoked by skills vs. directly by users?

**Workflow entry points:**
- What is the standard flow for starting a new feature? (e.g. scout → plan → orchestrate → implement)
- What is the standard flow for completing work? (e.g. commit, move issue status, post summary comment)

### 2. Discover the repo structure

**Packages / modules:**
- Is this a monorepo? List each package/module with its name and one-line purpose.
- What are the natural session boundaries? (packages that can be worked on independently)
- Which packages/modules have hard dependencies on others? (e.g. UI depends on API types)

**Dependency order for planning:**
- What must be done before what? (e.g. data schema → API → UI → tests)
- Are there shared type definitions that must be updated before consumers?

**Commit conventions:**
- What is the commit message format? (e.g. `type(scope): description (#issue)`)
- What types are used? (`feat`, `fix`, `refactor`, etc.)
- Is scope typically the package name, a feature area, or something else?
- Are issue references required?

**Issue / project management:**
- What tool is used? (GitHub Issues, Linear, Jira, etc.)
- Are there status transitions to perform when starting/completing work?
- Is there a standard comment format for implementation summaries?

### 3. Write the repo-specific skill

Copy `skills/generic-orchestrate/SKILL.md` to `.claude/skills/orchestrate/SKILL.md` (or wherever repo skills live) and apply these changes:

**Frontmatter:**
- Keep `name: orchestrate` and `disable-model-invocation: true`
- Keep `allowed-tools` as-is unless the repo uses different discovery tools

**Replace "Repo Context" section with concrete content:**

```markdown
## Repo Context

{Project name} is a {brief description}. Work is organized as follows:

### Available Skills

| Skill | Purpose |
|-------|---------|
| `{skill-name}` | {one-line description} |
{repeat for each}

### Package / Module Structure

| Package | Purpose |
|---------|---------|
| `{name}` | {one-line description} |
{repeat for each}

### Dependency Order

For planning session sequences, apply this order:
1. {e.g. Data schema / types}
2. {e.g. API / service layer}
3. {e.g. UI / frontend}
4. {e.g. Tests and documentation}

### Commit Conventions

Format: `{format string}` (e.g. `type(scope): description (#issue)`)
- Types: {list}
- Scope: {what it refers to}
- Issue refs: {required/optional, format}

### Starting and Completing Work

**Starting an issue:**
{paste the shell commands or steps for moving to In Progress}

**Completing an issue:**
{paste the shell commands or steps for moving to Testing/Done and posting summary}
```

**Update Plan Intake section:**
- In "Identify session boundaries", reference the actual package names as examples
- In "Order by dependency", reference the repo's actual layer names (not generic "schema/data/UI")

**Update Example Interactions:**
- Replace the generic `description field` example with a concrete example from this repo's domain
- Reference actual skill names in the example (e.g. "run `/scout-plan-execute` first, then bring the plan here")

### 4. Validate completeness

Verify the repo-specific skill covers:
- [ ] All available skills are listed with accurate descriptions
- [ ] All packages/modules are listed with one-line purposes
- [ ] Dependency order is documented (what must be done before what)
- [ ] Commit convention is shown with a concrete example
- [ ] Issue workflow steps are documented (start + complete)
- [ ] No `[To be filled in]` or `[populate based on repo]` placeholders remain
- [ ] The Plan Intake section references actual layer/package names, not generic placeholders
- [ ] Example interactions reference real skill names available in this repo
