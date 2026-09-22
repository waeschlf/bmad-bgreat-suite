# AgenticSRE

SRE, DevOps and Security agents for the [BMad Method](https://github.com/bmad-code-org/BMAD-METHOD) ecosystem.

> Fork of [petry-projects/bmad-bgreat-suite](https://github.com/petry-projects/bmad-bgreat-suite).
> Goal: extend the upstream production-readiness planning workflows with live operations
> workflows (incident triage, postmortem, problem and change management, on-call handoff)
> that work against real observability and ticketing tools via MCP.
> Module code `bgr` and skill IDs are kept unchanged so upstream changes can still be merged.

Three specialized agents — **Morgan** (SRE Lead), **Riley** (DevOps Lead), and **Sam** (Security Lead) — with ten guided workflows and a cross-agent operations review that produce production-readiness artifacts alongside your architecture planning.

## What's Included

### Agents

| Agent | Role | Expertise |
|-------|------|-----------|
| **Morgan** (SRE Lead) | Observability, incident response, SLO/SLI, reliability | Golden Signals, RED/USE methods, OpenTelemetry, error budgets, chaos engineering, blameless postmortems |
| **Riley** (DevOps Lead) | Infrastructure, CI/CD, deployment, platform engineering | Terraform/Pulumi, Kubernetes, GitOps, pipeline design, blue-green/canary deployments |
| **Sam** (Security Lead) | Threat modeling, compliance, security architecture | STRIDE/PASTA, SOC2/HIPAA/PCI-DSS/GDPR compliance mapping, supply chain security |

### Workflows

| Workflow | Menu Code | Owner | Output |
|----------|-----------|-------|--------|
| Create Observability Plan | `CO` | Morgan | Metrics, logging, tracing, SLOs, dashboards, alerting strategy |
| Create Incident Response Plan | `CR` | Morgan | Severity levels, runbooks, on-call procedures, postmortem templates |
| Create Disaster Recovery Plan | `CD` | Morgan | RTO/RPO targets, backup/restore, failover procedures, DR runbooks |
| Create Resilience Testing Plan | `CT` | Morgan | Steady-state hypotheses, failure scenarios, chaos experiment procedures |
| Create Chaos Game Day Plan | `CG` | Morgan | Hypothesis-driven experiments, blast radius controls, safety gates |
| Create Infrastructure Plan | `CI` | Riley | IaC strategy, environment topology, containers, networking |
| Create Pipeline Plan | `CP` | Riley | CI/CD stages, security scanning, deployment strategy, release gates |
| Create Capacity Plan | `CC` | Morgan + Riley | Growth modeling, auto-scaling, resource scaling, load testing |
| Create Cost Optimization Plan | `CF` | Riley + Morgan | FinOps strategy, right-sizing, reserved/spot, cost governance |
| Create Security Plan | `CS` | Sam | Threat model, security controls, auth strategy, compliance mapping |
| Operations Review | `OR` | Any agent | Cross-agent consistency review, coverage gaps, staleness checks |
| **Triage Incident** (live) | `IT` | Morgan | Evidence from observability tools, severity, ranked hypotheses, mitigation, status updates, ticket and postmortem stub |
| **Run Postmortem** (live) | `PM` | Morgan | Verified timeline and impact, blameless contributing factors, owned action items filed as tickets, published summary |
| **On-Call Handoff** (live) | `HO` | Morgan | Active incidents, firing and flapping alerts, changes, open items, prioritised handoff note, acknowledgement |

### Recommended Workflow Order

```
Architecture (bmm) --> Observability (CO) --> Incident Response (CR)
                   |                      --> Chaos Game Day (CG)
                   --> Infrastructure (CI) --> Pipeline (CP)
                   |                       --> Disaster Recovery (CD)
                   |                       --> Capacity Plan (CC)
                   |                       --> Cost Optimization (CF)
                   --> Security Plan (CS)

Incident Response (CR) --> Resilience Testing (CT)

After any workflows: Operations Review (OR)

During an incident: Triage Incident (IT) --> Run Postmortem (PM) --> planning workflows for the gaps found
                   (uses the Incident Response and Observability plans if present)
Every shift change: On-Call Handoff (HO) -- carries open items from handoff to handoff
```

Multiple tracks can run in parallel after the architecture is defined.

## Installation

### As a custom module (local path)

```bash
npx bmad-method install --custom-content /path/to/AgenticSRE/src
```

### As a Claude Code plugin

```bash
/plugin marketplace add waeschlf/bmad-bgreat-suite
/plugin install agentic-sre
```

### As an npm package (not published yet)

```bash
npm install agentic-sre
npx bmad-method install
# Select "AgenticSRE" from the module list
```

## Configuration

During installation, the module will ask:

- **bgr_artifacts** — Where to store operations planning artifacts (default: `_bmad-output/bgr-artifacts`)
- **bgr_maturity** — Your current operations maturity level (greenfield through advanced)
- **cloud_preference** — Primary cloud provider
- **container_orchestration** — Container platform (Kubernetes, ECS, Cloud Run, etc.)
- **bgr_incidents** — Where live incident records are stored (default: `_bmad-output/bgr-incidents`)
- **bgr_observability_tool** — Evidence source for triage (Datadog, Grafana stack, New Relic, ...), used via its MCP server
- **bgr_ticket_system** — Target for follow-up defects (Jira, Octane, ServiceNow, GitHub, Linear, none)

These settings shape how Morgan, Riley, and Sam tailor their guidance.

## Usage

### Wake an agent

From the BMad help menu, select:
- `MG` — Wake Morgan (SRE Lead)
- `RL` — Wake Riley (DevOps Lead)
- `SE` — Wake Sam (Security Lead)

### Run a workflow directly

Invoke any workflow by menu code:
- `CO` — Create Observability Plan
- `CR` — Create Incident Response Plan
- `CD` — Create Disaster Recovery Plan
- `CT` — Create Resilience Testing Plan
- `CG` — Create Chaos Game Day Plan
- `CI` — Create Infrastructure Plan
- `CP` — Create Pipeline Plan
- `CC` — Create Capacity Plan
- `CF` — Create Cost Optimization Plan
- `CS` — Create Security Plan
- `OR` — Operations Review
- `IT` — Triage Incident (live operations)
- `PM` — Run Postmortem (live operations)
- `HO` — On-Call Handoff (live operations)

### Cross-module collaboration

Morgan, Riley, and Sam can all invoke the Architecture (`CA`) and Implementation Readiness (`IR`) workflows from the core BMad Method module, bringing their operational expertise into those conversations.

## Module Structure

```
src/
  module.yaml              # Module configuration and install prompts
  module-help.csv          # Capability registry
  agents/
    bgr-agent-morgan-sre/  # SRE Lead persona
    bgr-agent-riley-devops/# DevOps Lead persona
    bgr-agent-sam-security/# Security Lead persona
  workflows/
    bgr-3-create-observability/
    bgr-3-create-incident-response/
    bgr-3-create-disaster-recovery/
    bgr-3-create-resilience-plan/
    bgr-3-create-chaos-gameday-plan/
    bgr-3-create-infrastructure/
    bgr-3-create-pipeline/
    bgr-3-create-capacity-plan/
    bgr-3-create-cost-optimization-plan/
    bgr-3-create-security-plan/
    bgr-4-incident-triage/   # Live operations: incident triage
    bgr-4-postmortem/        # Live operations: blameless postmortem
    bgr-4-oncall-handoff/    # Live operations: on-call shift handoff
  skills/
    bgr-ops-review/        # Cross-agent operations review
  templates/               # Shared production readiness checklist
```

Each workflow follows BMAD's micro-file architecture: a `workflow.md` orchestrator, sequential `steps/` for guided discovery, and `templates/` for output documents.

## Requirements

- [BMad Method](https://github.com/bmad-code-org/BMAD-METHOD) v6+
- Core module (required)
- BMad Method module (recommended — enables CA and IR cross-references)

## Attribution

Forked from [petry-projects/bmad-bgreat-suite](https://github.com/petry-projects/bmad-bgreat-suite) by Don Petry (MIT).

The upstream module incorporates ideas from these MIT-licensed projects in the BMad ecosystem:

- [Ricoledan/bmad-architecture-agent](https://github.com/Ricoledan/bmad-architecture-agent) — Platform Engineering expansion pack patterns (agent "Sam")
- [bacoco/BMad-Skills](https://github.com/bacoco/BMad-Skills) — Observability readiness skill structure and reference patterns

## Contributing

Contributions welcome. This module is designed to grow — new agents and workflows can be added following the patterns in `src/agents/` and `src/workflows/`.

See [CONTRIBUTING.md](./CONTRIBUTING.md). In short:

1. Create a feature branch
2. Follow existing conventions (SKILL.md, bmad-skill-manifest.yaml, workflow.md, steps/)
3. Run `bash tools/validate-skills.sh`
4. Submit a PR with a clear description

## License

MIT
