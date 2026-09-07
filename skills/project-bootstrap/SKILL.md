---
name: project-bootstrap
description: Use when starting a new non-frontend project or empty repository, or when asked to bootstrap, scaffold, or initialize devenv, AGENTS.md, or agents-nix for a backend, CLI, library, or services project. Also use for a new project that is not a UI, TypeScript app, bun, oxlint, or frontend stack.
---

# Project bootstrap

Scaffold devenv, AGENTS.md, and the agents-nix Skill Catalog in the project directory. Read [references/core.md](references/core.md) before changing devenv. Write AGENTS.md from [references/agents-md.md](references/agents-md.md), omitting the Frontend section.

A frontend, UI, TypeScript, bun, or oxlint project uses **project-bootstrap-frontend** instead.

## Steps

1. **Repo.** Work in the project directory. If `.git` is missing: `git init -b master`. Done when `git rev-parse --show-toplevel` is this directory and the default branch is `master`.

2. **devenv.** `devenv init` in this directory, then edit the generated files in place. Put `pkgs.git` in `packages`. Add agents-nix and the Skill Sources exactly as described in the core reference. Trust the project with `devenv allow`, then evaluate it with `devenv shell -- true`. Done when `devenv.nix`, `devenv.yaml`, and `devenv.lock` exist, there is no `.envrc`, and `devenv shell -- git --version` prints a version.

3. **AGENTS.md.** Copy the template, fill the project name, omit the Frontend section. Done when `AGENTS.md` exists at the repo root and states devenv-for-everything, Conventional Commits, and Context7-when-available.

4. **Skill Catalog.** Confirm `devenv.nix` imports agents-nix and declares the Emil Kowalski, Matt Pocock, and Ponytail Skill Sources. Leave every Agent Integration commented out. Done when the catalog evaluates, every input is pinned by `devenv.lock`, and neither `.agents/skills` nor `.claude/skills` is created.

## Common mistakes

| Move | Instead |
|------|---------|
| Write `devenv.nix` by hand or paste from another repo | `devenv init` in this directory |
| `devenv init` in a temp dir, then copy | Run it here |
| `devenv shell bash -c '...'` | `devenv shell -- <cmd>` |
| Enable an Agent Integration without the owner choosing it | Leave the examples commented out |
| Expect skill directories while integrations are disabled | Verify the catalog through devenv evaluation |
