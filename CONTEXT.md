# Project Bootstrap Skills

Skills that establish the shared conventions and capabilities of a new project.

## Language

**Project bootstrap**:
The process that establishes a new project’s development environment, instructions, and agent capabilities.

**Skill Catalog**:
The declared set of Skill Sources available to a project’s coding Agents.
_Avoid_: Installed skills

**Skill Source**:
A repository or standalone skill included in the Skill Catalog.
_Avoid_: Package

**Agent Integration**:
An explicit opt-in that exposes the Skill Catalog to one coding Agent. New projects declare available integrations but leave them disabled.
_Avoid_: Target, automatic installation

**Lint policy**:
A project-owned Oxlint ruleset loaded as a local plugin and enforced by the project’s lint configuration. Lint policy is not part of the Skill Catalog.
_Avoid_: Agent skill, agent capability
