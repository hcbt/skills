---
name: project-bootstrap-frontend
description: Use when starting a new frontend, UI, web, TypeScript, or bun project, or when asked to bootstrap a frontend with devenv, oxlint, oxfmt, or anti-slop. Also use when the new project will have a user interface.
---

# Frontend project bootstrap

**REQUIRED SUB-SKILL:** Follow [project-bootstrap](../project-bootstrap/SKILL.md) through every step first, including devenv, AGENTS.md, and APM. Then do the extras below. Devenv and APM rules stay in [project-bootstrap/references/core.md](../project-bootstrap/references/core.md). The AGENTS.md body, including the Frontend section, is [project-bootstrap/references/agents-md.md](../project-bootstrap/references/agents-md.md).

## Extras

1. **devenv.** Add `pkgs.bun` to `packages`. Set `languages.typescript.enable = true`. Do not enable `languages.javascript`. `devenv allow`, then `devenv shell -- bun --version`. Done when bun is on the devenv PATH.

2. **TypeScript project.** `devenv shell -- env BUN_AGENT_RULE_DISABLED=1 bun init -y`. Entry point is TypeScript. Set `allowJs` to `false` in `tsconfig.json`. Remove any `.js` / `.jsx` / `.mjs` / `.cjs` source bun wrote. If bun wrote `CLAUDE.md`, delete it; AGENTS.md is the instruction file. Done when `package.json` and `tsconfig.json` exist and there is no JavaScript source.

3. **oxlint and oxfmt.** `devenv shell -- bun add -D oxlint oxfmt`. Add `oxlint.config.ts` using `defineConfig` from `oxlint`. Enable devenv git-hooks that run `oxlint` and `oxfmt` on TypeScript (and TSX) files. If git-hooks is not already a devenv module, `devenv inputs add git-hooks github:cachix/git-hooks.nix --follows nixpkgs` then edit `devenv.nix`. Done when `devenv shell -- bunx oxlint --version` and `devenv shell -- bunx oxfmt --version` work and the hooks are in `devenv.nix`.

4. **anti-slop.** `devenv shell -- apm install dmmulroy/anti-slop --skill install-anti-slop`. Then follow the deployed **install-anti-slop** skill (`.agents/skills/install-anti-slop/`): copy the plugin, pin `@oxlint/plugins` to the same oxlint version, merge the plugin into `oxlint.config.ts`. Done when `tools/oxlint/anti-slop/` exists and `oxlint.config.ts` registers `anti-slop` with the generic rules at `"error"`.

5. **AGENTS.md.** Rewrite it from the template with the Frontend section included.

## Common mistakes

| Move | Instead |
|------|---------|
| `languages.javascript.enable` or `languages.javascript.bun.enable` | `pkgs.bun` plus `languages.typescript.enable` |
| Vendor anti-slop before oxlint is installed | oxlint first, then the install-anti-slop skill |
| `npx skills add dmmulroy/anti-slop` | `apm install dmmulroy/anti-slop --skill install-anti-slop` |
| JavaScript from bun init left in the tree | TypeScript only, `allowJs: false` |
