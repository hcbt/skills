---
name: project-bootstrap
description: Use when starting a new non-frontend project or empty repository, or when asked to bootstrap, scaffold, or initialize devenv, AGENTS.md, or apm for a backend, CLI, library, or services project. Also use for a new project that is not a UI, TypeScript app, bun, oxlint, or frontend stack.
---

# Project bootstrap

Scaffold devenv, AGENTS.md, and APM in the project directory. Read [references/core.md](references/core.md) before any devenv or apm command. Write AGENTS.md from [references/agents-md.md](references/agents-md.md), omitting the Frontend section.

A frontend, UI, TypeScript, bun, or oxlint project uses **project-bootstrap-frontend** instead.

## Steps

1. **Repo.** Work in the project directory. If `.git` is missing: `git init -b master`. Done when `git rev-parse --show-toplevel` is this directory and the default branch is `master`.

2. **devenv.** `devenv init` in this directory, then edit the generated files in place. Put `pkgs.git` and `pkgs.apm-cli` in `packages`. `devenv allow`. First `devenv shell -- true` writes `devenv.lock`. Done when `devenv.nix`, `devenv.yaml`, and `devenv.lock` exist, there is no `.envrc`, and `devenv shell -- git --version` prints a version.

3. **AGENTS.md.** Copy the template, fill the project name, omit the Frontend section. Done when `AGENTS.md` exists at the repo root and states devenv-for-everything, Conventional Commits, and Context7-when-available.

4. **APM.** From inside `devenv shell --`, `apm init -y` (or `apm-cli init -y` if that is the binary). Pin targets, add the three skill collections, rename the colliding `prototype` skills. `apm install`. Do not run `apm compile`. Done when `apm.yml` and `apm.lock.yaml` exist, `emil-prototype` and `matt-prototype` are deployed, and no unaliased `prototype/` skill directory remains.

## Common mistakes

| Move | Instead |
|------|---------|
| Write `devenv.nix` by hand or paste from another repo | `devenv init` in this directory |
| `devenv init` in a temp dir, then copy | Run it here |
| `devenv shell bash -c '...'` | `devenv shell -- <cmd>` |
| `curl \| sh` or Homebrew for apm | `pkgs.apm-cli` on the devenv PATH |
| `apm compile` | Stop after `apm install`; AGENTS.md is ours |
