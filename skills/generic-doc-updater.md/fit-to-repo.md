# Fitting generic-doc-updater to Your Repository

The `generic-doc-updater` agent works with any documentation structure. To maximize its effectiveness in your codebase, customize it by creating a repo-specific instance and mapping your documentation taxonomy.

## Quick Adaptation (5 minutes)

1. **Copy the agent** to your project:
   ```bash
   cp agents/generic-doc-updater.md .claude/agents/{your-project}-doc-updater.md
   ```

2. **Customize the taxonomy** section to match your actual docs:
   - List your actual doc files (`CLAUDE.md`, `README.md`, `docs/`, etc.)
   - Note where each type of info lives (schemas, API, workflows, components)
   - Add domain-specific categories if needed

   Example:
   ```markdown
   | `CLAUDE.md` | Project-wide rules, naming conventions | Always loaded |
   | `docs/api.md` | REST endpoints and responses | API questions |
   | `docs/db-schema.md` | Database tables and fields | Data modeling |
   | `src/{service}/README.md` | Service-specific setup | Working within a service |
   ```

3. **Adjust the "Read minimally" guidance** in the Process section:
   - If your project uses a different root-level rules file (not `CLAUDE.md`), say so
   - If there's a quick-reference doc (like `schema.md` or `API.md`), mention it
   - Example: "Start with `CLAUDE.md` and `docs/api-reference.md` for API questions"

4. **Optional: Add examples specific to your stack**:
   If your codebase has patterns unique to your stack (e.g., "exports must match the barrel pattern in `index.ts`"), add a note under "Diff-Driven Audit" or "Accuracy" to help the agent focus.

## What NOT to Customize

- The core audit checklist methodology—it's language, framework, and domain-agnostic
- The "Diff-Driven Audit" process—it works everywhere
- The response style—consistency is important

## Using the Adapted Agent

Once customized, invoke it with:
```
agent: doc-updater
```
or
```
/agent doc-updater
```

in your project, and it will use the customized taxonomy and guidance.

## Reference: Generic Taxonomy Categories

If you're unsure how to map your docs, here are the universal categories the generic agent checks for:

1. **Root-level rules** (conventions, patterns, env vars, architectural decisions)
2. **Onboarding** (setup, dependencies, quick start)
3. **Reference** (schemas, types, APIs, config options)
4. **Workflows** (multi-step processes, deployment, release)
5. **Component/module docs** (feature-specific guides)
6. **Design docs** (RFCs, implementation plans)

Every project has all six; the question is just where and what you call them.

## Examples

### TypeScript/Node.js Project
```markdown
| `CLAUDE.md` | Naming, exports, testing patterns | Always |
| `README.md` | Setup, npm scripts | Onboarding |
| `docs/api.md` | API routes and payloads | Building APIs |
| `docs/db.md` | Database schema and migrations | Data questions |
| `{package}/README.md` | Package-specific setup | Working in that package |
| `docs/deployment.md` | Build, deploy, release process | Deploying changes |
```

### Python/Django Project
```markdown
| `CLAUDE.md` | Code style, patterns, Django conventions | Always |
| `README.md` | Virtualenv, pip install, runserver | Onboarding |
| `docs/models.md` | Database models and fields | Schema questions |
| `docs/api.md` | Django REST endpoints | API questions |
| `docs/workflows/` | Deployment, testing, database migrations | Processes |
| `{app}/README.md` | App-specific models, views, templates | Working in an app |
```

### Full-Stack (Frontend + Backend)
```markdown
| `CLAUDE.md` | Architecture, monorepo structure, conventions | Always |
| `README.md` | Setup, dev server, build | Onboarding |
| `docs/api.md` | Backend REST/GraphQL schema and contracts | API design |
| `docs/schema.md` | Database and type definitions | Data modeling |
| `docs/frontend/components.md` | Component library and patterns | Building UI |
| `docs/workflows/deployment.md` | CI/CD, staging, production release | Deployment |
| `{service}/README.md` | Service-specific setup and structure | Working in a service |
```

---

Once you've customized the agent for your repo, you can invoke it for:
- Code review and documentation audit
- Post-feature-development doc updates
- Schema and API doc maintenance
- Catching drift before it accumulates
