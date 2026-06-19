# Task: Configure a Production CI Pipeline (Jenkins)

## Objective
Configure this project to build for production on our Jenkins instance by adding a declarative pipeline.

## Deliverables
1. Create a `Jenkinsfile` at the repository root.
2. Determine the environment variables and secrets required to build this project for production. Declare them in a single `environment {}` block at the top of the file so they are easy for a human to read and audit.
3. Implement the pipeline stages defined in **Pipeline Stages**.
4. Implement the post actions defined in **Post Actions**.
5. Clean your work to remove extraneous comments. At most, you may include short, one liner comments for complex concepts.

## Conventions
- Use a declarative `pipeline { ... }`.
- Pull all secrets from Jenkins credentials (`credentials('<id>')`); never hardcode secret values.
- Each stage should fail fast and surface a clear error message on failure.
- Keep stage logic language- and stack-agnostic; adapt the concrete commands to whatever this project uses.

## Pipeline Stages
| # | Stage | Purpose |
|---|-------|---------|
| 1 | **Checkout** | Pull the latest source for the commit being built. |
| 2 | **Preflight** | Validate prerequisites before doing real work: required secrets are present, external dependencies (e.g. a shared network) exist, and config is well-formed. Fail fast if not. |
| 3 | **Lint & Type-check** | Run static quality checks (linting, type/compile checks) in a clean, isolated environment to catch errors before deploying. |
| 4 | **Teardown** | Stop and remove the previous deployment, ensuring no leftover resources block the new one. |
| 5 | **Build & Deploy** | Build the release artifact (injecting any build-time config/secrets) and start the new version. |
| 6 | **Health Check** | Wait for the running app to report itself live; fail fast if it crashes or never becomes ready. |
| 7 | **Smoke Test** | Verify the deployment actually serves correctly by making a real request and asserting the expected response. |

## Post Actions
| Condition | Behavior |
|-----------|----------|
| **always** | Runs after every build regardless of outcome. Send a Discord notification with the build result (see **Discord Notification Spec**). |
| **failure** | Runs only when the build fails. Capture diagnostics (current state and recent logs) to aid debugging. |

## Discord Notification Spec

**Mechanism:** the Jenkins Discord Notifier plugin's `discordSend` step, inside `post { always { ... } }`. Posts one rich embed to a webhook URL on every build completion.

**Call:**
```groovy
discordSend(
  webhookURL: env.DISCORD_WEBHOOK,
  title: "📦 Build Alert: ${env.JOB_NAME} [Build #${env.BUILD_NUMBER}]",
  link: "${env.BUILD_URL}",
  result: "${currentBuild.currentResult}",
  description: discordDescription
)
```

**Parameters:**
- `webhookURL` — pulled from the secret credential `env.DISCORD_WEBHOOK`.
- `title` — `📦 Build Alert: <JOB_NAME> [Build #<BUILD_NUMBER>]`.
- `link` — `<BUILD_URL>` (makes the embed link back to Jenkins).
- `result` — `<currentBuild.currentResult>` (drives the embed's sidebar color).
- `description` — the message body below.

**Message body (`description`):**
```
**Status:** <emoji> <RESULT>
**Branch:** `<branch name, or "Main/Manual" if unavailable>`
**Duration:** :stopwatch: <human-readable build duration>

**Commits:**
<commit lines, or "No recent changes detected.">
```

**Field rules:**
- **Status emoji** — map from `currentBuild.currentResult`:
  - `SUCCESS` → `:green_circle:` 🟢
  - `FAILURE` → `:red_circle:` 🔴
  - any other result → `:yellow_circle:` 🟡
- **Branch** — the build's branch name; fall back to the literal `Main/Manual` when unavailable.
- **Duration** — the human-readable build duration, with the substrings `" and no weeks"` and `" and counting"` stripped out.
- **Commits** — one line per commit included in the build, formatted as `> <commit message> (by *<author display name>*)`, joined by newlines. If the build has no associated changes, replace the entire list with the literal text `No recent changes detected.`
