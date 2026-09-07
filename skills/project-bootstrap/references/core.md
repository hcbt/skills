# devenv and agents-nix

Facts used by both bootstrap skills. Commands run in the project directory.

## devenv

`devenv init` has to run **in the repo**. It writes `devenv.yaml`, `devenv.nix`, and appends `.gitignore`. It does not write `devenv.lock`; the first evaluation does (`devenv allow`, then `devenv shell -- true`). Commit that lock.

It refuses to overwrite an existing `devenv.nix` unless a TTY confirms. If the file already exists, skip init and edit in place.

`--option` flags on `init` do not persist into the generated files. Edit `devenv.nix` after.

No `.envrc`. Do not pass `--include-envrc`. Delete a leftover `.envrc`. Auto-activation is `eval "$(devenv hook zsh)"` in the user's shell rc plus `devenv allow` per project.

Always `devenv shell -- <cmd>`. Bare `devenv shell bash -c '...'` (no `--`) exits 0 and runs nothing.

After editing `devenv.nix`, `devenv allow` then confirm with a side effect (`devenv shell -- git --version`, not stdout from a bare `devenv shell`).

Starting `packages` list:

```nix
packages = [
  pkgs.git
];
```

## agents-nix

Add the module and three non-flake Skill Sources through devenv:

```sh
devenv inputs add agents-nix github:hcbt/agents-nix --follows nixpkgs
devenv inputs add skills-emilkowalski github:emilkowalski/skills
devenv inputs add skills-mattpocock github:mattpocock/skills
devenv inputs add skills-ponytail github:DietrichGebert/ponytail
```

Set `flake: false` on each `skills-*` input in `devenv.yaml`, then run `devenv update`. Keep agents-nix as a flake input and make its `nixpkgs` input follow this project's `nixpkgs`.

Merge this into the generated `devenv.nix`:

```nix
{ inputs, pkgs, ... }:

{
  imports = [ inputs.agents-nix.devenvModules.default ];

  packages = [
    pkgs.git
  ];

  agents.skills = {
    emilkowalski = inputs.skills-emilkowalski;
    mattpocock = inputs.skills-mattpocock;
    ponytail = inputs.skills-ponytail;
  };

  # Agent Integrations are opt-in. Enable only the Agents the owner chooses.
  # agents.antigravity-cli.enableSkillsIntegration = true;
  # agents.claude-code.enableSkillsIntegration = true;
  # agents.codex.enableSkillsIntegration = true;
  # agents.grok.enableSkillsIntegration = true;
  # agents.muse-code.enableSkillsIntegration = true;
  # agents.opencode.enableSkillsIntegration = true;
  # agents.pi-coding-agent.enableSkillsIntegration = true;
}
```

Pack skill ids are source-prefixed, so identically named skills do not collide. With every integration disabled, catalog evaluation writes no skill directory. Add `.agents/skills/` and `.claude/skills/` to `.gitignore` for the integrations an owner may enable later.

Run `devenv allow`, `devenv shell -- true`, and `devenv shell -- git --version`. The resulting `devenv.lock` pins agents-nix and every Skill Source.
