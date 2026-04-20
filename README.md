# Cursor Skills & Agents

A growing collection of custom **Cursor skills/agents** built to make development workflows faster, cleaner, and more automated.

These agents are designed to help with the boring stuff, the repetitive stuff, and the stuff that should already be one command away.

## Current Skills

### Project Skills (`.cursor/skills/`)

| Skill | Description |
| ----------- | ----------- |
| plan-issue | Plans the implementation for a GitHub issue by fetching details, exploring the codebase, and producing a step-by-step implementation plan. |
| pr-description | Generates a PR description from the current branch's code changes using a standardized template. |
| update-docs | Updates markdown documentation to reflect code changes on the current git branch. |
| draw-graph | Generates a Mermaid diagram showing classes changed on the current git branch and their immediate dependencies. |

### Personal Skills (`~/.cursor/skills/`)

| Skill | Description |
| ----------- | ----------- |
| drawgraph | Generates a Mermaid diagram of branch changes (personal copy, available across all projects). |
| update-docs | Updates markdown documentation to reflect code changes (personal copy, available across all projects). |

## Structure

```bash
.
├── plan-issue
├── pr-description
├── update-docs
├── draw-graph
└── ...