# Fitting generic-explorer to Your Repository

The `generic-explorer` agent is a research agent that produces structured findings for other specialist agents. To customize it for your project, define what downstream agents will consume its output and what your project's patterns and conventions are.

## Quick Adaptation (5 minutes)

1. **Copy the agent** to your project:
   ```bash
   cp agents/generic-explorer.md .claude/agents/{your-project}-explorer.md
   ```

2. **Customize the "Purpose" section**:
   - List the actual agents in your pipeline (e.g., if you don't have a data-architect, remove it)
   - Add any domain-specific agents unique to your project
   - Example for a backend-heavy project:
     ```markdown
     Your output is consumed by downstream specialists such as:
     - **API auditor** — needs route definitions, handler logic, DB queries
     - **Test strategist** — needs public API surfaces, error cases, integration points
     - **Security auditor** — needs user input handling, auth boundaries, dependency risks
     - **Database optimizer** — needs queries, indexes, migration patterns
     - **Documentation auditor** — needs current implementation vs. documented state
     ```

3. **Customize the "Output Format" sections**:
   - Remove sections your project doesn't use (e.g., remove "Component/UI Structure" for backend-only projects)
   - Add sections relevant to your project (e.g., "Queue/Task Structure" for a job queue system, "Deployment Pipeline" for infrastructure code)
   - Example additions for a data pipeline project:
     ```markdown
     ### Data Transformations
     - ETL steps and transformations
     - Input sources and output sinks
     - Error handling and retry logic

     ### Monitoring & Logging
     - What gets logged and at what level
     - Metrics exposed
     - Alert conditions
     ```

4. **Add a "Project Conventions" note** (optional):
   - Reference your project's CLAUDE.md or README to clarify naming, import patterns, or architectural rules
   - Example:
     ```markdown
     ## Project Conventions

     When researching, keep these conventions in mind (see CLAUDE.md):
     - Services are in `src/services/` and must export a default handler function
     - Database models are in `src/models/` with explicit TypeORM decorators
     - All exports must be re-exported through barrel files (`index.ts`)
     ```

5. **Optional: Add custom data model examples**:
   If your project has unique storage patterns, add examples
   - Example for a GraphQL project:
     ```markdown
     ### GraphQL Schema
     - Type definitions
     - Resolver implementations
     - Custom directives
     ```

## What NOT to Customize

- The research principles — they apply universally
- The overall process (define scope → map files → read → trace flows → compile)
- The response style (structured, factual, file:line references)
- The "Check project conventions" step in the process — always do this first

## Understanding the Role

The explorer is **not** a decision-maker. It's a researcher that:
- ✅ Reports facts: "This function has 12 call sites"
- ✅ Documents patterns: "All API handlers follow this shape"
- ✅ Flags issues: "This module is imported by 8 files but has no tests"
- ❌ Does NOT recommend: "You should refactor this"
- ❌ Does NOT judge: "This pattern is bad practice"

Let downstream agents draw their own conclusions from your findings.

## Examples by Project Type

### Backend REST API (Node.js/Python)
Remove: Component/UI Structure
Add or emphasize:
- API Surface (routes, handlers, client functions)
- Data Model (database schema, ORM models)
- Current Issues / Observations (N+1 queries, missing validation, etc.)

**Downstream agents:** API auditor, test strategist, security auditor, database optimizer, doc auditor

### Full-Stack Web Application (React + Node.js)
Keep all default sections
Customize Purpose to include: frontend agent, backend agent, integration tester
Emphasize: Component/UI Structure for frontend research, API Surface for backend research

**Downstream agents:** frontend agent, backend agent, test strategist, security auditor, doc auditor

### Data Pipeline / ETL System (Python)
Remove: Component/UI Structure, API Surface (unless there's a REST wrapper)
Add:
- Data Transformations (ETL steps, transformations, sources/sinks)
- Monitoring & Logging (what's tracked, alerts)
- Data Quality (validation rules, error handling)

**Downstream agents:** Data engineer, test strategist, monitoring agent, doc auditor

### Mobile App (React Native / Flutter)
Keep Component/UI Structure, remove API Surface (or minimize it)
Customize: Include mobile-specific concerns like state persistence, navigation patterns, platform-specific code

**Downstream agents:** Frontend agent, test strategist, platform integration agent

### Infrastructure as Code (Terraform / CloudFormation)
Remove: most default sections
Add:
- Resource Inventory (what's being provisioned)
- Dependencies (module dependencies, cross-stack references)
- Configuration (variables, environment differences)
- Security Boundaries (IAM roles, network zones)

**Downstream agents:** Infrastructure auditor, security auditor, drift detector, cost analyzer

---

Once customized, you can invoke the explorer with:
```
agent: explorer
```

Use it whenever you need to:
- Understand a feature area before implementing changes
- Prepare research output for specialist agents
- Map the current state of a codebase
- Identify cross-cutting concerns or dependencies before refactoring
