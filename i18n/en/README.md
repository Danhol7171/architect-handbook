---
source: README.md
source_hash: cf79734
---

# Architect Handbook

A methodology for software architects: roles, process, artifacts, templates, practices.
Scales from express audits to enterprise transformation.

> **Note:** English translation is in progress. The primary language of this project is Russian.
> See the [original documentation](../../README.md) for the most up-to-date content.

## Why

Architecture practice operates across projects of varying scale — from targeted audits to full IT landscape transformation. Architects (solution, technical, enterprise) work in different contexts but must speak the same language, use shared approaches, and deliver predictable results.

This handbook is a compact knowledge base that answers three questions:

1. **What does an architect do** — what tasks to solve at each project phase
2. **What does an architect produce** — what artifacts and in what form
3. **How does an architect do it** — approaches, tools, quality criteria

## Contents

| Section | Description |
| ------- | ----------- |
| [Core Standard](docs/core-standard.md) | Mandatory core: rigor levels (L0-L3), artifact matrix, Definition of Done |
| [Roles & Competencies](docs/roles.md) | 5 architect profiles, responsibilities, competency matrix |
| [Process](docs/process.md) | 8-phase architectural process (from presale to support) with AI augmentation |
| [Artifacts](docs/artifacts.md) | 30+ architectural documents with templates and examples |
| [Practices](docs/practices.md) | 18 practices: ADR, review, NFR, fitness functions, API, data, EDA, security, FinOps, platforms, AI |
| [Tools](docs/tools.md) | Notations, tooling profiles by level, AI tools |
| [Playbooks](docs/playbooks.md) | 7 playbooks: audit, discovery, delivery, platform, modernization, cloud migration, transformation |
| [Governance Charter](docs/governance-charter.md) | Decision rights, ARB, escalation, SLA on decisions, waivers |
| [Quality Gates](docs/quality-gates.md) | CI/CD gates, review checklists, phase gates |
| [Roadmap](docs/roadmap.md) | 3 implementation horizons, operating model, knowledge management, metrics |

## Templates

The [`templates/`](templates/) directory contains ready-to-use artifact templates.

## Principles

- **Compactness over completeness.** 20 pages people read beat 200 pages they don't
- **Scales with the project.** Minimal set for small projects, full structure for large ones
- **Grounded in reality.** Every artifact and practice is battle-tested on real projects
- **Tool-agnostic.** The methodology describes "what", not "which tool"

## How to use

1. Start with [Core Standard](docs/core-standard.md) — determine the rigor level for your project (L0-L3)
2. Review [roles](docs/roles.md) — who is needed on the project
3. Walk through the [process](docs/process.md) — what phases and what happens at each
4. Grab the [templates](templates/) you need — don't create artifacts from scratch

## License

Distributed under the [CC BY-SA 4.0](../../LICENSE) license.

## Author

**Dmitry Petrakov** — [GitHub](https://github.com/dpetrakov)
