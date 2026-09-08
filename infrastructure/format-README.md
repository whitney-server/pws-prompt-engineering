# Task: Format README

## 1. Objective

Generate or refactor the repository's `README.md` to be concise, low-bloat, highly scannable, and developer-focused, ending with a simple operational runbook.

## 2. Requirements

### Content & Tone Principles
* **Low-Bloat & Direct:** Zero marketing prose, redundant introductory headers, or generic badges that add visual noise. Descriptions must be CONCISE AND DIRECT. 
* **Applicability:** Only include sections relevant to the project (e.g., omit build steps for zero-build scripts, omit package publishing steps if it is a standalone service).
* **Single Source of Truth:** Defer deep architectural details to `.claude/DESIGN.md` or dedicated docs rather than cluttering the README.
* **Keep it High Level:** Design and other concepts should be described at a higher level, e.g. don't describe all interfaces, describe what interfaces exist and how they manipulate data.

### Standard Structure & Sections

1. **Title & One-Liner Description**
   * Clear project name followed by a single-sentence summary explaining its core function.

2. **Key Features & Capability Highlights**
   * A short bulleted list (3–6 items) of core application capabilities and key differentiators.

3. **Local Dev Prerequisites**
   * Minimum required runtimes, tools, and dependencies (e.g., Node.js >= 18, Docker Compose >= 2.0, PostgreSQL 15).
   * Simple explanation of how to install dependencies via a script or command (e.g. pip install -r requirements.txt).

4. **Configuration & Environment Variables**
   * Table or brief list of essential `.env` variables, default values, and required secrets.

5. **Releases & Versioning (GitHub Specific, optional)**
   * Explanation of how releases are distributed (e.g., GitHub Releases, published container tags, or package registries).
   * Brief note on versioning conventions (e.g., Semantic Versioning `vX.Y.Z`) and release asset artifacts.

6. **Operational Runbook (Final Section)**
   * A sequential, copy-pasteable CLI runbook covering standard day-to-day operations:
     * **Local Setup & Development:** Clone, install, configure, and start in dev mode.
     * **Testing & Linting:** Commands to execute test suites and quality checks.
     * **Production Build & Run:** Container build and execution commands.
     * **Common Operations:** Database migrations, log tailing, or cleanup commands.

## 3. Implementation Steps

1. Inspect the codebase, package manifests, and existing documentation to gather real operational commands.
2. Draft the `README.md` following the low-bloat structural template.
3. Verify that all terminal commands in the runbook are accurate and copy-pasteable.
4. Eliminate any redundant sections, wordy descriptions, or excessive badge links.
