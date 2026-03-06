# Fit Generic Test Strategist to Your Repo

Adapt `agents/generic-test-strategist.md` to your specific project in ~5 minutes.

## Quick Adaptation

1. Copy `generic-test-strategist.md` into your project's `.claude/agents/` directory
2. Rename to `test-strategist.md` (or keep the prefix — your call)
3. Customize the sections below
4. Done — invoke with your agent runner

## What to Customize

### 1. Opening Line
Replace the generic intro with your project context:
```markdown
# Before
You are a test strategy specialist.

# After
You are a test strategy specialist for the Acme billing platform.
```

### 2. Test Hierarchy — Reorder by Domain Risk
The four tiers (data integrity, contract, business logic, interaction) are defaults. Reorder or rename them based on what matters most in your project:

| Project Type | Highest Priority |
|---|---|
| Financial / billing | Data integrity, then business logic (calculations) |
| API platform | Contract tests, then data integrity |
| UI-heavy SPA | Interaction/behavior, then contract |
| Data pipeline | Data integrity, then transformation logic |
| Auth / security | Business logic (permissions), then contract |

### 3. Critical Paths — Add Your Specifics
Replace the generic bullets with your actual critical code paths:
```markdown
# Example for a Python/Django e-commerce app
- Order placement mutations that write to PostgreSQL
- Price calculation and discount application
- Inventory reservation and release logic
- Payment gateway request/response handling
- Email template rendering with dynamic data
```

### 4. Domain-Specific Test Concerns — Fill This In
This section ships as a commented-out template. Replace with your real concerns:

```markdown
# Example: fintech app
- Currency rounding: are calculations done in cents/minor units?
- Timezone handling: do scheduled transactions fire at the right local time?
- Idempotency: can the same request be safely retried?
- Rate precision: do floating-point rates accumulate errors?
```

```markdown
# Example: internationalized app
- Unicode normalization: do searches handle composed vs decomposed forms?
- RTL layout: do bidirectional text inputs render correctly?
- Date/number formatting: do locale-specific formats parse and display correctly?
```

```markdown
# Example: infrastructure / DevOps
- Terraform state drift: do plan and apply produce consistent results?
- Secret rotation: do services pick up new credentials without restart?
- Failover: does the backup region accept traffic within SLA?
```

### 5. Language & Framework (important)
The generic agent is language-agnostic. Add your stack's specifics:

```markdown
# Example additions for TypeScript/Node
- Schema validation: Zod/Joi schemas parse edge-case inputs correctly
- Test type includes: Unit (vitest) / Integration (supertest) / E2E (playwright)
- Type-level tests: complex generics and conditional types

# Example additions for Python
- Test type includes: Unit (pytest) / Integration (pytest + fixtures) / E2E (selenium/playwright)
- Type checking: mypy strict mode passes on all tested modules
- Fixture patterns: factory_boy or pytest fixtures for test data

# Example additions for Go
- Test type includes: Unit (testing) / Integration (testcontainers) / E2E (custom)
- Race detection: tests pass under -race flag
- Benchmark tests for hot-path functions

# Example additions for Rust
- Test type includes: Unit (#[test]) / Integration (tests/) / Property (proptest)
- Unsafe code: all unsafe blocks have dedicated test coverage
- Compile-time guarantees: verify that invalid states don't compile (compile_fail tests)
```

## What NOT to Customize

These are the agent's core methodology — changing them weakens it:

- **Analysis Process** (5-step: read source → read tests → map gaps → assess risk → recommend)
- **Risk-based prioritization** (the principle, not the specific order)
- **Recommendations format** (what/why/type/scenarios/assertion structure)
- **Response style** (prioritized lists, concrete descriptions, complexity estimates)
- **The four-tier test hierarchy** (you can reorder and rename, but keep the concept of tiered prioritization)

## Project Type Quick-Start

### Backend REST API (Node/Python/Go/Ruby)
- Emphasize: contract tests, data integrity, error handling
- Add: middleware tests, auth boundary tests
- De-emphasize: interaction/behavior tests

### Full-Stack Web App
- Keep all four tiers
- Split: separate API-side and UI-side critical paths
- Add: integration tests spanning frontend → backend

### Mobile App
- Emphasize: interaction/behavior, offline state handling
- Add: platform-specific edge cases (iOS/Android differences)
- De-emphasize: contract tests (unless you own the API)

### Data Pipeline / ETL
- Emphasize: data integrity, transformation correctness
- Add: idempotency tests, schema evolution tests, data quality assertions
- De-emphasize: interaction/behavior tests

### CLI Tool / Library
- Emphasize: contract tests (public API surface), business logic
- Add: argument parsing edge cases, backward compatibility
- De-emphasize: interaction/behavior tests

## Invocation

```bash
# As a Claude Code agent
claude "Analyze test coverage for the auth module" --agent test-strategist

# Or reference in another agent/skill
Use the test-strategist agent to assess coverage gaps in {module}.
```
