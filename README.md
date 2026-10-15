# DeployBuddy

**Your brutally honest pre-deployment sanity checker. Know before you ship.**

DeployBuddy turns project evidence into a clear deployment assessment. The web UI includes a browser-local project folder scanner for common source/configuration patterns, while the CLI, VS Code extension, and GitHub Action provide the repository-oriented assessment surfaces.

## Product surfaces

- **VS Code:** deployment checks, Git guardrails, Release Readiness, repository Release Gates, local history, and `@deploybuddy` Chat. Optional explanations use the VS Code Language Model API.
- **CLI:** `deploybuddy check`, `status`, `git`, `doctor`, `framework`, `checks`, `changes`, `readiness`, `release`, and local history/baseline commands.
- **GitHub Action:** shared Core assessment, annotations, outputs, summary, optional single PR comment, and independent opt-in risk/readiness gates plus configured release-gate enforcement.
- **Shared packages:** `@deploybuddy/core` owns deterministic platform-independent intelligence; `@deploybuddy/node-adapter` collects local Node/Git evidence.

## Browser project scan MVP

Open the web app and select **Project Scan** to choose a project folder. The browser scanner identifies common project manifests, checks readable source/configuration files for heuristic patterns (possible hard-coded credentials, debug output, TODO/FIXME markers, HTTP URLs, empty exception handlers, and selected SQL/security patterns), shows evidence and suggested owners/fixes, and exports a JSON report. Common generated folders such as `node_modules`, `.git`, `dist`, `target`, and `build` are skipped. Files are processed locally in the browser.

**Important limitations:** this browser scan does not execute project builds or tests, query vulnerability databases, or deploy applications. Those checks remain `UNKNOWN`, not `PASS`. Pattern matches can be false positives or miss real issues. For actual build/test execution, use the local CLI/backend runner; uploaded project builds should run in an isolated environment. The scanner is a useful first assessment layer, not a guarantee that a release is safe.

The previous questionnaire-based risk calculator remains available at `/quick-check`.

## Example

```text
Release Readiness: BLOCKED
Risk: 78% — DO NOT DEPLOY
Confidence: 91% — HIGH
Policy: PASS
Change Attention: HIGH

Why:
- 2 validation checks failed
- Branch is behind upstream
- Database migrations changed

Next:
Fix failing checks, synchronize the branch, and review the migration changes.
```

Risk, confidence, policy, change attention, readiness, and release gates are separate values. AI wording never calculates or changes deterministic outcomes. See [Concepts](docs/concepts.md) for the canonical model.

## Documentation

- [Repository configuration and JSON Schema](docs/configuration.md)
- [CLI usage, JSON contract, and exit codes](docs/cli.md)
- [VS Code extension](docs/vscode.md)
- [GitHub Action](docs/github-action.md)
- [Privacy](docs/privacy.md)
- [Architecture and public APIs](docs/architecture.md)
- [Release process](docs/releasing.md)

## Development

```sh
npm install
npm run validate:platform
npm run release:check
npm run dev # browser demo
```

`release:check` builds and packages local artifacts only. It never publishes, tags, or pushes. Public package versions are independent; the published VS Code 0.1.0 extension is prepared as 0.2.0, while Core, Node adapter, and CLI remain at 0.1.0 for their initial public package line. Read [Version and compatibility policy](docs/architecture.md) before changing contracts.
