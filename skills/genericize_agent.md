---
name: genericize-agent
description: "Strip repo-specific details from an agent prompt to make it reusable across projects. Use when an agent file contains hardcoded project names, file paths, port numbers, dependency names, or architecture details that tie it to a single codebase."
user_invocable: true
---

# Genericize Agent

Take an agent prompt file and make it portable across repositories by removing project-specific details while preserving the agent's core expertise and structure.

## Steps

1. **Read the agent file** provided by the user
2. **Identify repo-specific content** — scan for each category below
3. **Apply transformations** — replace or restructure each instance
4. **Preserve structure** — keep the frontmatter, section headings, checklist format, and response style intact
5. **Add a discovery section** if the agent needs runtime context — instruct it to discover project details from the codebase rather than having them hardcoded

## What to strip

| Category | Example (before) | Replacement (after) |
|---|---|---|
| **Project names** | "for the Pelikanas Lithuanian vocabulary learning system" | Remove or replace with generic role statement |
| **Hardcoded file paths** | `app/server.ts`, `app/src/api/dictionary.ts` | Generic descriptions like "the main server file" or "the API client module" |
| **Port numbers** | "port 3001", "ports 3100–3200" | Remove or generalize to "the configured port" |
| **Dependency/type names** | `AppDeps`, `createXxxRoutes` | Generic descriptions like "shared dependencies", "route factory functions" |
| **Specific response shapes** | ```{ success: true, data: T }``` | "the project's standard response envelope" |
| **Utility function names** | `safeFetch`, `apiJson<T>` | "typed fetch wrappers" or "client utility functions" |
| **`[populate based on repo]` placeholders** | Leftover TODOs from partial genericization | Either remove the section or convert to a discovery instruction |
| **Project-specific sections** | "Pelikanas-Specific Checks" with hardcoded patterns | Fold reusable checks into the general section; drop the rest |

## What to preserve

- **Frontmatter** (`name`, `description`, `model`, `color`) — keep the structure, update description if it references the project
- **Core principles** — keep the domain expertise and best practices (these are universal)
- **Checklist items** — keep but reword to reference "the project's conventions" instead of specific patterns
- **Process steps** — keep and add "Discover architecture" as step 1 if the agent needs repo context
- **Response style** — keep as-is (formatting preferences are project-independent)
- **Examples in description** — keep the structure but ensure they don't assume a specific project

## Key judgment calls

- If a section is 100% project-specific with no reusable content, **delete it entirely**
- If a section mixes universal principles with project-specific examples, **keep the principles, strip the examples**
- If the agent relies on knowing project structure at runtime, **add an Architecture Discovery section** that tells it to examine the codebase first
- Hardcoded values that become generic descriptions should still be **concrete enough to be actionable** — "the server entry point" is better than "relevant files"
