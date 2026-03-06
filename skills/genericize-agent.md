# Genericize Agent Skill

Automate the process of converting a domain-specific agent into a generic, reusable agent that works across multiple projects.

## Usage

```bash
/genericize-agent /path/to/source-agent.md [--output-dir .claude/agents] [--name custom-name]
```

**Parameters:**
- `source-agent` (required): Path to the agent file to genericize (e.g., `@/Users/.../pelikanas/.claude/agents/explorer.md`)
- `--output-dir` (optional): Directory to place the genericized agent. Default: `.claude/agents/`
- `--name` (optional): Name for the new generic agent. Default: `generic-{original-name}`

**Examples:**
```bash
# Genericize explorer.md, output to .claude/agents/generic-explorer.md
/genericize-agent @/Users/felixdulys/cde/pelikanas/.claude/agents/explorer.md

# Customize output location
/genericize-agent @/path/to/agent.md --output-dir ./skills/agents/

# Customize the name
/genericize-agent @/path/to/agent.md --name universal-reviewer
```

## What This Skill Does

1. **Reads the source agent** and identifies domain-specific references
2. **Removes domain specificity**:
   - Project/product names (e.g., "Pelikanas Lithuanian vocabulary learning system" → "your codebase")
   - Domain-specific libraries or tools (e.g., "lentele component catalog" → "component library")
   - Proprietary terminology and examples
   - Project-specific architectural patterns (unless universally applicable)
3. **Generalizes the agent's scope**:
   - Converts hardcoded downstream agents to customizable examples
   - Converts proprietary data models to generic ones
   - Converts specific file structures to flexible taxonomies
4. **Creates the genericized agent** in the output directory
5. **Auto-generates a `fit-to-repo.md` skill** at `{output-dir-parent}/skills/{generic-name}/fit-to-repo.md` that explains how to customize it for a specific project

## Genericization Rules

### What Gets Changed
- **Project/Product Names**: "Pelikanas" → "your project"
- **Domain-Specific Libraries**: "lentele components" → "shared primitives/UI library"
- **Specific Examples**: Remove concrete examples tied to one project; replace with "[your project]" or "[domain-specific tool]"
- **Hardcoded Downstream Agents**: Convert to customizable lists with examples
- **Proprietary Concepts**: Map to generic equivalents (e.g., "vocabulary card" → "domain entity")

### What Stays the Same
- **Core Methodology**: The agent's fundamental approach and principles
- **Output Format**: Structure remains consistent
- **Research/Audit Principles**: The core values and processes
- **Response Style**: Tone, formatting, documentation standards
- **Process Steps**: The logical sequence of work

### Special Cases
- **UI/Component Agents**: Generalize component library references, keep UI methodology
- **API/Data Agents**: Generalize storage and framework references, keep architectural thinking
- **Specialist Agents**: Remove domain expertise references, keep specialist role definition

## Output Structure

```
.claude/agents/generic-{name}.md
  (genericized agent definition)

.claude/skills/generic-{name}/
  └── fit-to-repo.md
      (guide for customizing the generic agent for a specific repo)
```

If you use `--output-dir` for the agent, the skill will be created at:
```
{output-dir-parent}/skills/generic-{name}/
  └── fit-to-repo.md
```

## The fit-to-repo.md Skill

Auto-generated alongside the generic agent, this guide includes:

1. **Quick Adaptation** (5 minutes):
   - How to copy and customize for the target repo
   - What sections to modify
   - What NOT to change

2. **Project Type Examples**:
   - Customization patterns for different project types
   - Example downstream agents
   - Example output sections

3. **Understanding the Agent's Role**:
   - What it does (fact-finding, structure analysis, etc.)
   - What it doesn't do (recommendations, judgments)

4. **Reference**: How to invoke the customized agent once adapted

## Tips for Successful Genericization

- **Preserve the agent's core identity**: If it's a "researcher," keep that. If it's an "auditor," preserve that role.
- **Be generous with customization hooks**: The more the user can customize, the more reusable the agent is.
- **Provide examples**: The fit-to-repo.md should include 3-5 project type examples.
- **Document unknowns**: If the agent makes assumptions about the codebase, note them so users can adjust.

## Example: Genericizing an Explorer Agent

**Before (domain-specific):**
```markdown
You are a codebase explorer for the Pelikanas system...
Output is consumed by frontend-reviewer, api-auditor, data-architect...
- **Component Structure** — lentele component catalog...
```

**After (generic):**
```markdown
You are a codebase explorer and research specialist...
Your output is consumed by downstream specialists such as:
- frontend-reviewer, api-auditor, data-architect, etc. (customize as needed)
- Include/remove sections based on your project type...

### Component/UI Structure (if UI involved)
- Shared primitives or UI library used
```

---

Once created, the generic agent can be customized for any project using the generated `fit-to-repo.md` guide.
