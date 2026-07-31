# hcbt/skills

A Claude Code plugin marketplace with one plugin: **hcbt-skills**.

The repository root *is* the plugin. [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json)
lists a single entry whose source is `./`, so
[`skills/`](skills) and [`output-styles/`](output-styles) install together from
one clone.

## Install

Add the marketplace, then install the plugin:

```bash
/plugin marketplace add hcbt/skills
```

```bash
/plugin install hcbt-skills@hcbt
```

`hcbt` is the marketplace name. `hcbt-skills` is the plugin name.

To manage the plugin with nix (home-manager), add the same repository as a
marketplace and set the `enabledPlugins` key. One marketplace, one plugin key.

## What the plugin ships

### Skills

Claude loads a skill on demand, when the task matches its description.

- **[ste-writing](skills/ste-writing/SKILL.md)** — write or rewrite one piece of
  prose in ASD-STE100 Simplified Technical English. Two modes: strict for
  procedures, runbooks, safety text, and error messages, STE-flavored for
  general prose. It covers docs, READMEs, pull-request text, commit bodies,
  release notes, and comments. It does not cover code, identifiers, or command
  syntax.

### Output style

An output style replaces Claude's default response style for the whole session.
The user selects it once, and it stays on.

- **[ste-writing](output-styles/ste-writing.md)** — the same STE rules as the
  skill, applied to every answer instead of on demand. Two flags shape it:
  `keep-coding-instructions: true` keeps the normal software-engineering
  behavior, and `force-for-plugin: true` makes the style outrank the prose style
  of any other plugin.

The skill and the output style hold the same rules on purpose, and they differ
in reach. The output style is the always-on route: select it, and every answer
follows STE. The skill is the on-demand route: it triggers per task, so it fits
a session that runs a different style but still needs one document in STE.

## Layout

```
.claude-plugin/marketplace.json   the marketplace, listing one plugin
skills/<name>/SKILL.md            one directory per skill
skills/<name>/references/         optional supporting files for a skill
output-styles/<name>.md           one file per output style
```

To add a skill, create `skills/<name>/SKILL.md` with `name` and `description`
frontmatter. The description decides when Claude loads the skill, so write it
for the trigger, not for the reader.
