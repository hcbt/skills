# hcbt/skills

A Claude Code plugin marketplace with one plugin: **hcbt-skills**.

The repository root *is* the plugin. [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json)
lists a single entry whose source is `./`, so files under `skills/` install
from one clone.

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

The plugin ships no skills today. See **Layout** to add one.

## Layout

```
.claude-plugin/marketplace.json   the marketplace, listing one plugin
skills/<name>/SKILL.md            one directory per skill
skills/<name>/references/         optional supporting files for a skill
```

To add a skill, create `skills/<name>/SKILL.md` with `name` and `description`
frontmatter. The description decides when Claude loads the skill, so write it
for the trigger, not for the reader.
