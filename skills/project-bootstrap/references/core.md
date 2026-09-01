# devenv and APM

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
  pkgs.apm-cli
];
```

The nixpkgs attribute is `apm-cli`. The binary is `apm` or `apm-cli` depending on the pin. `devenv shell -- apm --version` or `devenv shell -- apm-cli --version`. Use whichever exists. If nixpkgs `apm-cli` is too old for git-longhand `alias:` or `--target grok-build`, overlay a newer build from GitHub. Do not curl the installer onto the host.

## APM

After devenv has `apm` on PATH:

1. `devenv shell -- apm init -y` (drop `-y` if this pin rejects it). That writes `apm.yml`.
2. Set:

```yaml
targets:
  - grok-build
  - agent-skills
```

A fresh tree has no harness markers. Without `targets`, `apm install` exits 2.

3. `emilkowalski/skills` and `mattpocock/skills` both ship a directory named `prototype`. Discover the skill directory names from each repo at bootstrap time (do not hardcode a stale list). On each collection entry, set `skills:` to every name except `prototype`. Add path-scoped deps so the clash deploys under aliases:

```yaml
dependencies:
  apm:
    - git: https://github.com/emilkowalski/skills.git
      skills:
        - emil-design-eng # plus every other emilkowalski skill except prototype
    - git: https://github.com/emilkowalski/skills.git
      path: skills/prototype
      alias: emil-prototype
    - git: https://github.com/mattpocock/skills.git
      skills:
        - tdd # plus every other mattpocock skill except prototype
    - git: https://github.com/mattpocock/skills.git
      path: skills/engineering/prototype
      alias: matt-prototype
    - git: https://github.com/DietrichGebert/ponytail.git
```

Replace the placeholder `skills:` names with the full discovered lists. An empty `skills:` list installs nothing. Ponytail has no `prototype` clash; install the repo as a whole.

4. `devenv shell -- apm install`
5. Confirm `emil-prototype` and `matt-prototype` exist under the deployed skills root (`.agents/skills/` for `agent-skills`). Confirm there is no unaliased `prototype/` directory. If a collection still deployed one, the `skills:` subset is wrong; fix `apm.yml` and install again.

Do not run `apm compile`. It writes root context files and would fight AGENTS.md.
