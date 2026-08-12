# Raven Use Cases

A curated collection of real-world Raven integrations, reusable workflows, and community showcases.

This repository helps people move from discovering what Raven can do to understanding a workflow and reproducing it. It documents the Raven-specific integration layer and points to upstream projects instead of duplicating or maintaining their source code.

## Repository structure

| Directory | Purpose |
| --- | --- |
| [`integrations/`](integrations/) | Documents how Raven works with other projects, platforms, and tools. |
| [`recipes/`](recipes/) | Contains reusable Raven workflows, Markdown processes, prompts, skills, and runnable examples. |
| [`showcases/`](showcases/) | Highlights community outcomes, demos, posts, and videos. |
| [`templates/`](templates/) | Provides the required structure for new integrations, recipes, and showcases. |
| [`assets/`](assets/) | Stores repository-owned images and other lightweight supporting assets. |

## Content types

### Integrations

An integration page explains how Raven works with another project or platform. It should include:

- the problem the integration solves;
- representative use cases;
- the relationship and data flow between Raven and the upstream project;
- setup and usage steps;
- Raven-specific workflows, skills, prompts, or configuration;
- links to the upstream repository and relevant documentation;
- the version or commit used during verification;
- the last verification date and maintenance owner.

Upstream source code should not be copied into this repository by default. If an integration requires changes to an upstream project, maintain those changes in a separate fork and link both the fork and the original repository from the integration page.

### Recipes

A recipe is a reusable way to complete a task with Raven. Recipes may include:

- Markdown research workflows;
- Raven skills;
- prompt and configuration templates;
- automation procedures;
- sample inputs and outputs.

Every recipe should state the problem, prerequisites, inputs, workflow, expected output, limitations, author, and source.

### Showcases

A showcase presents a result or story created with Raven. It may reference a social post, demo, or video, but it should not be only a bare external link.

Each showcase should include:

- a concise description of what was created;
- the author and original source;
- a summary of the workflow or outcome;
- an authorized preview image when available;
- a link to a reproducible recipe when one exists.

External media remains hosted at its original source unless redistribution permission has been granted.

## Maturity levels

Every entry must declare one of the following states:

| Status | Meaning |
| --- | --- |
| `inspiration` | Demonstrates an idea or result but is not expected to be reproducible. |
| `documented` | Includes enough detail to follow, but has not been recently verified by a maintainer. |
| `verified` | Has been run successfully and includes a recent verification date. |

## Contribution standards

1. Start from the matching file in [`templates/`](templates/).
2. Keep one use case per directory.
3. Document the Raven-specific workflow instead of duplicating an upstream project.
4. Link original sources and credit all authors and maintainers.
5. Include version and verification information for technical instructions.
6. Clearly separate official and community-contributed material.
7. Do not include private data, credentials, or material without redistribution rights.
8. Prefer concise, reproducible examples over product claims.

## Initial integration

- [Raven × RPA](integrations/rpa/)

This page is an initial placeholder and will be expanded with verified workflows, setup instructions, and examples.

## Licensing and attribution

Contributors retain responsibility for ensuring they have the right to submit code, text, images, and other assets. Third-party projects remain subject to their own licenses. Linking to a project does not change its license or imply endorsement.

A repository-wide license will be added after the intended licensing model for code, documentation, and contributed media has been confirmed.
