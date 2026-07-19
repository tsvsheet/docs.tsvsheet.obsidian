---
title: Security
---

The plugin runs inside your vault, so its security posture is deliberately strict. This page lists every measure in place and links each one's **live results**; the repository's [SECURITY.md](https://github.com/tsvsheet/tsvsheet.obsidian/blob/main/SECURITY.md) is the canonical copy and includes how to report a vulnerability privately.

## Design constraints

- **Zero third-party runtime dependencies.** The shipped bundle contains only the plugin's own code plus [@tsvsheet/tsvsheet](https://github.com/tsvsheet/tsvsheet.js); the compute engine is the tsvsheet Go implementation compiled to WebAssembly, running in-process with no I/O of its own.
- **No network access.** The plugin makes no requests; the engine and all assets load from the plugin's own directory.
- **No HTML from data.** Grid values — including formula results and error strings — render via `textContent` only, so vault content can never inject markup or script.
- **Your files are never rewritten with computed output.** The `.tsvt` view persists only the engine's canonical source serialization — formulas, comments, and the trailing newline survive every edit exactly.

## Automated scanning (live results)

| Measure | Cadence | Live results |
| --- | --- | --- |
| [CodeQL](https://github.com/tsvsheet/tsvsheet.obsidian/blob/main/.github/workflows/codeql.yml) static analysis with the `security-extended` query pack | every push/PR + weekly | [workflow runs](https://github.com/tsvsheet/tsvsheet.obsidian/actions/workflows/codeql.yml) · [code scanning alerts](https://github.com/tsvsheet/tsvsheet.obsidian/security/code-scanning) |
| [`npm audit`](https://github.com/tsvsheet/tsvsheet.obsidian/blob/main/.github/workflows/verify.yml) dependency vulnerability scan, all severities, dev dependencies included | every push/PR + weekly | [workflow runs](https://github.com/tsvsheet/tsvsheet.obsidian/actions/workflows/verify.yml) |
| [OSSF Scorecard](https://github.com/tsvsheet/tsvsheet.obsidian/blob/main/.github/workflows/scorecard.yml) supply-chain posture rating | every push + weekly | [public scorecard](https://scorecard.dev/viewer/?uri=github.com/tsvsheet/tsvsheet.obsidian) · [workflow runs](https://github.com/tsvsheet/tsvsheet.obsidian/actions/workflows/scorecard.yml) |
| [Dependabot](https://github.com/tsvsheet/tsvsheet.obsidian/blob/main/.github/dependabot.yml) vulnerability alerts and version updates (npm + GitHub Actions) | continuous + weekly | [dependency updates](https://github.com/tsvsheet/tsvsheet.obsidian/network/updates) |
| Secret scanning with push protection | continuous | enforced by GitHub on every push |

## Repository constraints

- Every commit is **cryptographically signed**; the organization enforces verified signatures on push.
- The repository's own workflows grant **least-privilege permissions** and pin third-party actions **by commit SHA**.
- The plugin gate — typecheck, lint, tests, production build — runs on every push and pull request ([verify](https://github.com/tsvsheet/tsvsheet.obsidian/actions/workflows/verify.yml)), and the repository's badges surface each workflow's current state on the [project README](https://github.com/tsvsheet/tsvsheet.obsidian#readme).
- Vulnerabilities are reported through [GitHub private vulnerability reporting](https://github.com/tsvsheet/tsvsheet.obsidian/security/advisories/new), never public issues.
