---
name: project-bootstrap-frontend
description: Use when starting a new frontend, UI, web, TypeScript, or bun project, or when asked to bootstrap a frontend with devenv, oxlint, oxfmt, or anti-slop. Also use when the new project will have a user interface.
---

# Frontend project bootstrap

**REQUIRED SUB-SKILL:** Follow [project-bootstrap](../project-bootstrap/SKILL.md) through every step first, including devenv, AGENTS.md, and the Skill Catalog. Then do the extras below. Shared devenv and agents-nix rules stay in [project-bootstrap/references/core.md](../project-bootstrap/references/core.md). The AGENTS.md body, including the Frontend section, is [project-bootstrap/references/agents-md.md](../project-bootstrap/references/agents-md.md).

## Extras

1. **devenv.** Add `pkgs.bun` to `packages`. Set `languages.typescript.enable = true`. Do not enable `languages.javascript`. `devenv allow`, then `devenv shell -- bun --version`. Done when bun is on the devenv PATH.

2. **TypeScript project.** `devenv shell -- env BUN_AGENT_RULE_DISABLED=1 bun init -y`. Entry point is TypeScript. Set `allowJs` to `false` in `tsconfig.json` and add `node_modules/` to `.gitignore`. Remove any `.js` / `.jsx` / `.mjs` / `.cjs` source bun wrote. If bun wrote `CLAUDE.md`, delete it; AGENTS.md is the instruction file. Done when `package.json` and `tsconfig.json` exist, dependencies are ignored, and there is no JavaScript source.

3. **oxlint and oxfmt.** `devenv shell -- bun add --dev --exact oxlint oxfmt`. Add `oxlint.config.ts` using `defineConfig` from `oxlint`. Enable devenv git-hooks that run `oxlint` and `oxfmt` on TypeScript (and TSX) files. If git-hooks is not already a devenv module, `devenv inputs add git-hooks github:cachix/git-hooks.nix --follows nixpkgs` then edit `devenv.nix`. Done when `devenv shell -- bunx oxlint --version` and `devenv shell -- bunx oxfmt --version` work and the hooks are in `devenv.nix`.

4. **anti-slop.** Read and follow [references/anti-slop.md](references/anti-slop.md). Materialize the pinned ruleset with devenv `files`, then register it as a local Oxlint plugin. Done when `tools/oxlint/anti-slop/` is writable project-owned source with its upstream license, `@oxlint/plugins` exactly matches the Oxlint version, every generic rule is `"error"`, the Effect rule is enabled when `effect` is a direct dependency, and the project's full check passes.

5. **AGENTS.md.** Rewrite it from the template with the Frontend section included.

## Common mistakes

| Move | Instead |
|------|---------|
| `languages.javascript.enable` or `languages.javascript.bun.enable` | `pkgs.bun` plus `languages.typescript.enable` |
| Materialize anti-slop before oxlint is installed | oxlint first, then seed and configure anti-slop |
| Treat anti-slop as an Agent Skill | Seed its source with devenv and load it through Oxlint |
| JavaScript from bun init left in the tree | TypeScript only, `allowJs: false` |
