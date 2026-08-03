# hcbt/skills

A Claude Code plugin marketplace with one plugin: **hcbt-skills**.

The repository root *is* the plugin. [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json)
lists a single entry whose source is `./`, so `skills/` and
[`output-styles/`](output-styles) install together from one clone.

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

### Output style

An output style replaces Claude's default response style for the whole session.
The user selects it once, and it stays on.

- **[ste-writing](output-styles/ste-writing.md)** — every ASD-STE100 Simplified
  Technical English rule, plus a fixed response shape, applied to every answer.
  It covers docs, READMEs, pull-request text, commit bodies, release notes,
  comments, and chat. It does not cover code, identifiers, or command syntax.
  Two flags shape it: `keep-coding-instructions: true` keeps the normal
  software-engineering behavior, and `force-for-plugin: true` makes the style
  outrank the prose style of any other plugin.

The style is the only route on purpose. A skill held the same rules until
2026-08-03, and it kept its own copy of a mode split that let general prose
relax the STE dictionary. Two copies drift, and Claude resolved the
contradiction arbitrarily. The style now carries every rule on its own.

### Skills

The plugin ships no skills today. See **Layout** to add one.

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
