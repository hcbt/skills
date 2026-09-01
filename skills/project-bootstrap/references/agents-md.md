# AGENTS.md template

Write this file to the project root as `AGENTS.md`. Replace `<project>` with the directory name.

**project-bootstrap:** copy through Context7. Stop before `## Frontend`.

**project-bootstrap-frontend:** copy the whole file.

```markdown
# <project>

## devenv

All dependencies, services, tests, git hooks, and project tools come from devenv. Run every command as `devenv shell -- <cmd>`. Start services with `devenv up`. Run the project's tests the way devenv defines them (`devenv test` or the test task in `devenv.nix`).

Do not use host Python, Node, bun, or other host toolchains. Do not add a `.envrc`. Trust the project with `devenv allow`. After changing `devenv.nix` or `devenv.yaml`, confirm with a side effect, not a bare `devenv shell`.

## APM

Agent skills and other agent primitives are declared in `apm.yml` and installed with `devenv shell -- apm install` (or `apm-cli` if that is the binary). Commit `apm.yml` and `apm.lock.yaml`. Do not copy skills into the tree by hand.

## Git

Default branch is `master`. Commit messages follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).

## Context7

When the Context7 MCP is available in the session, use it for library, framework, SDK, API, CLI, and cloud-service documentation, including API syntax, setup, and version-specific behavior. Training-data knowledge of those libraries is stale. If Context7 is not available, continue without it; do not fail the task for that reason.

## Frontend

Runtime is bun. Language is TypeScript. Do not add JavaScript source (no `.js`, `.jsx`, `.mjs`, `.cjs`). `allowJs` stays false. Package scripts and devenv hooks call `bun`, `oxlint`, and `oxfmt` through `devenv shell --`.

Lint with oxlint (`oxlint.config.ts`). Format with oxfmt. The anti-slop plugin is vendored at `tools/oxlint/anti-slop/` and registered in the oxlint config; do not reinstall it as an npm package.
```
