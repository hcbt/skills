# hcbt/skills

A plugin marketplace with one plugin: **hcbt-skills**. Skills live under `skills/`
and install into Claude Code, Grok, Codex, and anything else that reads
[Agent Skills](https://agentskills.io).

The repository root *is* the plugin. Each harness has its own index pointing at
`./`:

| Harness | Index |
| --- | --- |
| Claude Code | [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) |
| Grok | [`.grok-plugin/marketplace.json`](.grok-plugin/marketplace.json) |
| Codex | [`.codex-plugin/plugin.json`](.codex-plugin/plugin.json) and [`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json) |

## Install

**Claude Code**

```bash
/plugin marketplace add hcbt/skills
/plugin install hcbt-skills@hcbt
```

**Grok**

```bash
grok plugin marketplace add hcbt/skills
grok plugin install hcbt-skills --trust
```

**Codex**

```bash
codex plugin marketplace add hcbt/skills
```

Then install **hcbt-skills** from that marketplace. `hcbt` is the marketplace
name. `hcbt-skills` is the plugin name.

## What the plugin ships

### Skills

- **[project-bootstrap](skills/project-bootstrap/SKILL.md)** — devenv, AGENTS.md, and APM for a new non-frontend project.
- **[project-bootstrap-frontend](skills/project-bootstrap-frontend/SKILL.md)** — the same, plus bun, TypeScript, oxlint, oxfmt, and anti-slop.

## Layout

```
.claude-plugin/marketplace.json    Claude Code marketplace
.grok-plugin/marketplace.json      Grok marketplace
.codex-plugin/plugin.json          Codex plugin manifest
.agents/plugins/marketplace.json   Codex marketplace
skills/<name>/SKILL.md             one directory per skill
skills/<name>/references/          optional supporting files for a skill
```

To add a skill, create `skills/<name>/SKILL.md` with `name` and `description`
frontmatter. The description decides when an agent loads the skill, so write it
for the trigger, not for the reader.
