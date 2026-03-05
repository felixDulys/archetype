---
name: fit-api-auditor-to-repo
description: "Customize the generic api-auditor agent to a specific repository by discovering its API architecture and filling in concrete details. Use when onboarding the api-auditor agent to a new codebase."
user_invocable: true
---

# Fit API Auditor to Repo

Take the generic api-auditor agent template and produce a repo-specific version by discovering the project's actual API architecture.

## Inputs

- The generic agent template at `agents/generic-api-auditor.md`
- The current repository's codebase

## Steps

### 1. Discover the API architecture

Search the repo to answer each of these questions. Collect concrete file paths, names, and patterns.

**Server structure:**
- What is the main server entry point? (e.g., `src/server.ts`, `app/index.ts`)
- What framework is used? (Express, Fastify, Hono, Next.js API routes, etc.)
- How are routes mounted? (in-process router, file-based routing, controller classes)
- What port(s) does the server run on?

**Route organization:**
- Where do route definitions live? (single file, per-feature directories, route factories)
- What is the route factory/module pattern? (e.g., `createXxxRoutes(deps): Router`, controller classes, file-based)
- Are there any route prefixes applied at mount time?

**Response contract:**
- What is the standard response envelope? Copy the actual TypeScript types or shapes.
- Are there shared response helper functions? (e.g., `sendSuccess()`, `sendError()`)

**Client API layer:**
- Where does the frontend API client live? (e.g., `src/api/`, `lib/fetchers/`)
- What utility functions exist for fetching? (names and signatures)
- How are client functions organized? (by domain, single file, auto-generated)

**Dependency injection:**
- How are shared resources (DB, cache, auth) passed to route handlers?
- What is the deps type called? Copy its interface.

**Error handling:**
- Is there global error middleware?
- What is the standard error response shape?

**Health checks:**
- Does a health/status endpoint exist? What path?

**Port assignments:**
- List all known ports and what they serve.

### 2. Write the repo-specific agent

Copy `agents/generic-api-auditor.md` to `agents/api-auditor.md` (or whatever name the user prefers) and apply these changes:

**Frontmatter:**
- Update `name` to drop the "generic-" prefix
- Update `description` to reference the project name if known

**Replace "Architecture Discovery" with "Architecture Context":**
- Remove the discovery instructions — the agent no longer needs to figure these out at runtime
- Replace with a concrete section documenting what you found in step 1, structured as:

```markdown
## Architecture Context

{Project name} uses {framework} with {routing strategy}:

- **Server entry point** (`{path}`, port {N}) — {how routes are mounted}
- **Route definitions** (`{pattern}`) — {factory/module pattern with actual function signatures}
- **Frontend API client** (`{path}`) — {organization and key utility names}

### Response Envelope

{Paste the actual TypeScript types}

### Client Utilities

- `{functionName}(args)` — {description}
{repeat for each}
```

**Update Core Principles with concrete references:**
- In "Contract Consistency" — reference the actual envelope shape (e.g., `{ success, data }`)
- In "Client-Server Alignment" — reference the actual client directory path
- In "Route Module Integrity" — reference the actual deps type and factory pattern
- In "Error Handling" — reference the actual error envelope shape

**Update Review Checklist:**
- Replace generic phrasing with project-specific terms (e.g., "the project's standard response envelope" → "`{ success: true, data: T }`")

**Add a project-specific section:**
- Add a section named "{Project}-Specific Checks" covering:
  - The exact route factory pattern with example
  - Server routing details (mount points, prefix handling)
  - Frontend API layer file organization
  - Port assignments table
  - Any project-specific conventions discovered

**Update Process:**
- Remove "Discover architecture" as step 1 (it's now baked in)
- Start with "Identify scope"

### 3. Validate completeness

Verify the repo-specific agent covers:
- [ ] All server entry points are documented
- [ ] Route factory/module pattern is shown with real function signatures
- [ ] Response envelope types are pasted verbatim
- [ ] Client utility functions are listed by name
- [ ] Deps type/interface is documented
- [ ] Port assignments are listed
- [ ] No `[populate these based on repo]` placeholders remain
- [ ] No "discover" or "examine" language remains in the Architecture section — everything is concrete
