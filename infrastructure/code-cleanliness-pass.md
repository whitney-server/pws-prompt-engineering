# Task: Code Cleanliness & Hygiene Standard

## 1. Objective
Refactor the application codebase to be readable, maintainable, and scalable by removing redundant code, and breaking down monolithic structures into reusable modules. The refactored code should strictly adhere to clean code principles, standard style guidelines, and industry standard structure for AI-assisted development.

## 2. Requirements

### Separation of Concerns
* Decouple business logic, data access, network/HTTP handling, and UI/presentation layers.
* Move database queries out of API handlers/controllers into dedicated repository or service modules.
* Extract configuration parameters, environment variables, and constant values into centralized config files or constants modules rather than hardcoding them within logic files.

### Comment Hygiene
* Remove obvious, redundant, or outdated comments (e.g., commenting what a line of code plainly states, such as `// increment i`).
* Delete commented-out code blocks; rely on Git history for code retrieval instead.
* Restrict comments strictly to **single-line comments** (`//` or `#`) explaining:
  * Non-obvious domain logic or conceptually complex algorithms.
  * Workarounds for framework edge cases or external API quirks.
  * Essential context or warnings for future maintainers.

### Component & Function Modularization
* Break down monolithic functions or massive components into small, single-responsibility, reusable functions or components.
* Enforce the Single Responsibility Principle (SRP): each function, class, or module should have one reason to change.
* Identify duplicate code blocks or repeated logic patterns across files and extract them into shared utility functions or abstract base patterns.
* Keep function parameters to a minimum (ideally 3 or fewer); group related parameters into options objects or dedicated data types where necessary.

### Code Style & Language Guidelines
* Adhere strictly to the standard style guide and idiomatic patterns of the primary language:
  * **JavaScript / TypeScript:** Standard JS / Airbnb style guide, consistent `const`/`let` usage, early returns, async/await over raw promise chains.
  * **Python:** PEP 8 compliance, explicit typing hints, snake_case naming, clear docstrings for public APIs.
  * **Go:** Standard `gofmt` style, explicit error handling, short camelCase variable names.
  * **Java / C#:** Standard PascalCase/camelCase conventions, object-oriented encapsulation, interface-driven design.
* Enforce clear, intent-revealing variable and function names (avoid single-letter names except in standard loop counters).

### Defensive Code Hygiene
* Enforce early return / guard clauses to eliminate deeply nested `if-else` blocks (aim for a maximum cyclomatic complexity/nesting depth of 2-3 levels).
* Ensure strict error handling: handle errors explicitly rather than catching and swallowing exceptions silently.
* Remove unused imports, dead functions, unreferenced variables, and leftover debugging statements (`console.log`, `print`, `debugger`).

### Claude Context Hygiene (`.claude/` Directory)
* Clean up and standardize the repository root `.claude/` directory according to official Claude memory and context best practices:
  * Remove temporary artifacts, obsolete prompts, cached model outputs, and redundant docs.
  * Ensure secret credentials, local `.env` variables, and internal user data are omitted and verified via `.gitignore`.
* Maintain the standardized suite of knowledge context files in `.claude/`:
  * **`CLAUDE.md` (Primary Root Entry Point):** Project overview, tech stack, directory structure, build/test/lint commands, workflow guardrails, and links to secondary knowledge files.
  * **`GOALS.md`:** Core objectives, scope limits, target audience, priorities, and explicit non-goals.
  * **`DESIGN.md`:** Architectural overview, system component interaction, design trade-offs, and data-flow diagrams.
  * **`IMPLEMENTATION.md`:** Codebase evolution history, refactoring milestones, active technical debt, and limitations.
  * **`GLOSSARY.md`:** Concise definitions of domain-specific terminology, acronyms, and custom concepts.
* Keep all context files scannable, dense with facts, and completely free of conversational or marketing fluff. Use explicit `TODO:` tags for any missing details requiring human input.

---

## 3. Implementation Steps

1. Run the project's linter and static analysis tools to identify style violations, dead code, and high-complexity files.
2. Refactor complex controllers or monolithic files by extracting business logic into services and utilities.
3. Clean up comments and unused code across all modified files.
4. Clean up the `.claude/` folder and populate or update `CLAUDE.md`, `GOALS.md`, `DESIGN.md`, `IMPLEMENTATION.md`, and `GLOSSARY.md`.
5. Run the existing test suite to ensure all functionality and edge cases remain intact without regressions.

---

## 4. Verification Commands

Provide verification steps in the PR or documentation:

```bash
# 1. Run standard linter / formatter check
npm run lint          # Node.js/TypeScript
# or: flake8 / black --check .  # Python
# or: golangci-lint run          # Go

# 2. Verify .claude/ directory structure and missing flags
ls -la .claude/
grep -rn "TODO:" .claude/

# 3. Run test suite to verify no functional regressions
npm test              # Node.js/TypeScript
# or: pytest                     # Python
# or: go test ./...              # Go
