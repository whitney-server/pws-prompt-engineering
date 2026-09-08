# Configure Claude Dotfiles for Project
Create and populate a project-level `.claude/` directory in the repository root to establish a standardized knowledge context for Claude and other AI agents.

## 1. Objective
Create and populate a project-level `.claude/` directory in the repository root to establish a standardized knowledge context for Claude and other AI agents.

## 2. Requirements

### Context Source Rules
* Ground all content strictly on actual inspection of repository files, package definitions, configuration settings, and git history.
* Prohibit speculative statements, placeholder defaults, or marketing filler.
* Insert explicit `TODO:` markers for missing context, unstated requirements, or unverified architectural details.
* Keep text clear, objective, and scannable using concise bullet points and direct technical prose.

### Knowledge File Schema
Populate the `.claude/` directory with the following standardized markdown files:

1. **`.claude/CLAUDE.md` — Primary Entry Point**
   * **Project Overview:** A single concise paragraph summarizing the project's purpose.
   * **Tech Stack & Tooling:** Primary programming languages, runtime environments, frameworks, and package managers.
   * **Directory Structure:** High-level project layout mapping top-level folders with a single-line description per folder.
   * **Developer Workflow Commands:** Exact CLI commands to install dependencies, run development servers, build, test, lint, and format.
   * **Agent Guidelines & Guardrails:** Coding standards, naming conventions, formatting preferences, git branch naming, and commit message styles.
   * **Knowledge Base Links:** Explicit references linking to `GOALS.md`, `DESIGN.md`, `IMPLEMENTATION.md`, and `GLOSSARY.md`.

2. **`.claude/GOALS.md` — Scope & Objectives**
   * Statement of core capabilities and supported use cases.
   * Target audience and operational environment.
   * Explicit non-goals, out-of-scope features, and current operational priorities.

3. **`.claude/DESIGN.md` — Architecture & Key Decisions**
   * System architecture, component breakdowns, and interaction patterns.
   * Key design trade-offs, rationale behind major architectural choices, and rejected alternatives.
   * Data-flow diagrams or dependency overviews where applicable.

4. **`.claude/IMPLEMENTATION.md` — Evolution & Technical Debt**
   * Historical evolution summarized from git history and changelogs.
   * Key development milestones, major refactors, and technology migrations.
   * Current known limitations, technical debt, and areas actively being modified.

5. **`.claude/GLOSSARY.md` — Domain & Project Terminology**
   * Definitions for project-specific terms, acronyms, custom protocols, and domain concepts required by external agents.

## 3. Implementation Steps

1. Check for the existence of `.claude/` in the repository root; create the directory if missing.
2. Inspect codebase manifests (`package.json`, `pyproject.toml`, `go.mod`, Dockerfiles, etc.), git commit history, and existing docs.
3. Generate the required knowledge files (`CLAUDE.md`, `GOALS.md`, `DESIGN.md`, `IMPLEMENTATION.md`, `GLOSSARY.md`) adhering strictly to facts.
4. Insert `TODO:` flags wherever repository details are missing or incomplete.
5. Review generated documents to ensure formatting consists of short, factual, highly scannable sentences.

## 4. Verification & Reporting

Upon creating or updating the `.claude/` knowledge base, generate a terminal summary output:

```bash
# Example Output Summary Format:
==================================================
.claude/ Knowledge Base Generation Summary
==================================================
Files Created/Updated:
 - .claude/CLAUDE.md
 - .claude/GOALS.md
 - .claude/DESIGN.md
 - .claude/IMPLEMENTATION.md
 - .claude/GLOSSARY.md

Flags Requiring Human Attention:
 [!] .claude/GOALS.md: line 12 - TODO: Define target user persona
 [!] .claude/DESIGN.md: line 24 - TODO: Document database migration strategy
==================================================
