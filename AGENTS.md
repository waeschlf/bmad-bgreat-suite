# AgenticSRE — Agent Development Standards

> This file defines project-specific conventions for the AgenticSRE module
> (a fork of [petry-projects/bmad-bgreat-suite](https://github.com/petry-projects/bmad-bgreat-suite)).

## Project Overview

AgenticSRE is a BMAD Method extension module providing three
specialized AI agents (Morgan/SRE, Riley/DevOps, Sam/Security), ten
guided workflows for production readiness planning, and a cross-agent
operations review skill. The codebase is
content-only: markdown workflow definitions, YAML configuration, and
output templates.

- **Type:** BMAD Method extension module (content-only: markdown + YAML)
- **Agents:** Morgan (SRE), Riley (DevOps), Sam (Security)
- **Workflows:** 10 guided workflows for production readiness planning
- **Skills:** `bgr-ops-review` cross-agent operations review
- **Output:** Planning artifacts saved to `{bgr_artifacts}/` directory

### Key Files

- `src/module.yaml` — Module configuration and installation prompts
- `src/module-help.csv` — Complete skill registry with dependencies
- `src/agents/` — Agent persona definitions
- `src/workflows/` — Guided multi-step workflows
- `src/skills/` — Standalone skills (operations review)

## Repository Structure

```
src/
├── agents/           # Agent persona definitions (SKILL.md + manifest)
├── workflows/        # Guided multi-step workflows
│   └── {name}/
│       ├── SKILL.md              # Entry point
│       ├── bmad-skill-manifest.yaml
│       ├── workflow.md           # Orchestrator
│       ├── steps/                # Sequential step files
│       └── templates/            # Output document templates
├── skills/           # Standalone skills (bgr-ops-review)
├── templates/        # Shared templates (production readiness checklist)
├── module.yaml       # Module configuration
└── module-help.csv   # Skill registry
```

## Development Standards

### Skill File Conventions

- Every SKILL.md MUST have YAML frontmatter with `name` and `description` fields
- Every workflow directory MUST contain: SKILL.md, bmad-skill-manifest.yaml, workflow.md, steps/, templates/
- Every agent and workflow MUST be registered in `module-help.csv`
- Step files are numbered sequentially: `step-01-init.md`, `step-02-*.md`, etc.
- Step files MUST include mandatory execution rules block
- Step files MUST present [C]ontinue / [R]evise menus and halt for user input

### Content Quality

- Markdown tables must use standard syntax (`| col | col |`) — no double pipes
- YAML frontmatter must be valid (parseable by standard YAML parsers)
- Template variables use `{{variable}}` or `{variable}` syntax consistently
- Config variables reference `{bgr_artifacts}`, `{bgr_maturity}`, `{communication_language}`, etc.

### Cross-Workflow Consistency

- Workflows that reference other workflow outputs must use the discovery pattern (sharded-first logic)
- Quality gates in step-05-validation must be verifiable and specific
- Cross-workflow coherence checks must reference actual artifacts, not hypothetical ones
- The production readiness checklist must be updated by every workflow's validation step

### Module Registration

- New agents must be added to module-help.csv with correct phase, dependencies, and menu code
- New workflows must specify `after:` dependencies accurately
- Module.yaml must reflect the correct agent/workflow counts
- New skills must be listed in `.claude-plugin/marketplace.json`

### Security

- No secrets, API keys, or credentials in any file
- No permission bypass directives (e.g., flags that skip tool authorization checks)
- Agent configs must not include overly permissive tool authorizations
- SKILL.md files must not instruct agents to bypass security controls

## CI Checks

All PRs must pass:
- **CI Validate** — YAML validation, module structure, module-help.csv consistency, `tools/validate-skills.sh`
- **Secret scan** — gitleaks over full history

Run `bash tools/validate-skills.sh` locally before pushing.
