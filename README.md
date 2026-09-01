# hcbt/skills

A Grok plugin marketplace with one plugin: **hcbt-skills**.

The repository root *is* the plugin. [`.grok-plugin/marketplace.json`](.grok-plugin/marketplace.json)
lists a single entry whose source is `./`, so files under `skills/` install
from one clone.

## Install

Add the marketplace, then install the plugin:

```bash
grok plugin marketplace add hcbt/skills
```

```bash
grok plugin install hcbt-skills --trust
```

`hcbt` is the marketplace name. `hcbt-skills` is the plugin name.

To pick up a newly installed plugin, press `r` in the Plugins tab or start a
new session.

## What the plugin ships

### Skills

- **[project-bootstrap](skills/project-bootstrap/SKILL.md)** — devenv, AGENTS.md, and APM for a new non-frontend project.
- **[project-bootstrap-frontend](skills/project-bootstrap-frontend/SKILL.md)** — the same, plus bun, TypeScript, oxlint, oxfmt, and anti-slop.

## Layout

```
.grok-plugin/marketplace.json     the marketplace, listing one plugin
skills/<name>/SKILL.md            one directory per skill
skills/<name>/references/         optional supporting files for a skill
```

To add a skill, create `skills/<name>/SKILL.md` with `name` and `description`
frontmatter. The description decides when Grok loads the skill, so write it
for the trigger, not for the reader.
