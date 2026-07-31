# skills

Personal Claude Code skills. Develop them here, then install each one into
`~/.claude/skills/` with a symlink:

```
ln -s ~/src/projects/skills/skills/<skill-name> ~/.claude/skills/<skill-name>
```

## Skills

- **ste-writing** — rewrite prose (docs, READMEs, pull-request text, error
  messages, release notes, comments) into ASD-STE100 Simplified Technical
  English. Two modes: strict for procedures and safety text, STE-flavored for
  general prose. It does not apply to code, identifiers, or command syntax.
