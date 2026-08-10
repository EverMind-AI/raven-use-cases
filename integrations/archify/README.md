---
title: Raven × Archify — Verified Repository Architecture Map
type: integration
status: documented
maintainer: ""
upstream_repository: https://github.com/tt-a1i/archify
verified_version: v2.13.0
last_verified: 2026-08-10 (example artifact and source evidence verified)
tags: [architecture, visualization, repository-analysis, skill, html]
---

# Raven × Archify: Create a verified architecture map from a real repository

[简体中文](README.zh-CN.md)

## Overview

This use case shows how Raven can analyze a real code repository and use the
Archify Skill to produce a polished, interactive, and verifiable system map.

The deliverable is not a screenshot or a generic flowchart. Archify creates a
typed JSON specification, validates it with deterministic checks, and delivers
a self-contained HTML file that supports navigation, search, source evidence,
route exploration, themes, and export.

> Verification note: the included Raven architecture artifact was delivered by
> Archify v2.13.0 with all 9 showcase checks passing, 0 errors, and 0 warnings.
> Raven successfully analyzed its installed source and produced the first
> candidate, but that candidate needed a focused schema/layout repair before
> the deterministic final delivery. The page therefore remains `documented`
> until a fresh Raven session completes the authoring loop without intervention.

## Included example: Raven runtime architecture

[Open the interactive HTML](example/raven-runtime.html) ·
[Typed JSON](example/raven-runtime.architecture.json) ·
[Validation receipt](example/raven-runtime.validation.json) ·
[Standalone SVG](example/raven-runtime.svg)

![Raven runtime architecture](example/raven-runtime-preview.png)

The example maps Raven itself—not EverOS—as the subject. Its primary path is
`User → Channel Layer → Session Manager → Agent Loop → Provider Layer → Model
APIs`, with Context Engine, Tools/MCP/Skills, Memory Engine, Proactive Engine,
Evolver, and Local Workspace shown as supporting runtime branches.

Evidence is pinned to Raven v0.1.10 at commit
`52c76a56020e295a514dd4717fe0d20b0ae7956c`. The Archify receipt verifies 18
source references and byte-identifies both the JSON specification and delivered
HTML. The PNG/SVG are reader previews; the self-contained HTML is the canonical
interactive artifact.

## Why Raven and Archify work well together

Raven provides the agent workflow: it reads the repository, identifies the
important runtime story, invokes local tools, responds to feedback, and keeps
the task context. Archify provides the constrained diagram workflow: typed
source, five technical diagram types, deterministic validation, atomic HTML
delivery, and an interactive viewer.

The integration flow is:

```text
Repository or system description
  → Raven analyzes evidence and chooses a bounded story
  → Archify Skill produces typed JSON IR
  → Archify validates schema, layout, routes, labels, and artifact integrity
  → Archify atomically delivers a self-contained HTML map
  → The user explores, reviews, exports, and asks Raven for focused revisions
```

## Official Raven installation method

Yes: Archify is installed in Raven as a workspace Skill.

The generic command shown at the top of the Archify README is:

```bash
npx skills add tt-a1i/archify -g
```

However, the official documentation explicitly says that Raven is **not** a
target of the current agent switcher. For Raven, use the packaged ZIP instead:

1. Download the official [`archify.zip`](https://github.com/tt-a1i/archify/blob/v2.13.0/archify.zip).
2. Extract it into `~/.raven/workspace/skills`.
3. Confirm that the final path is exactly:

   ```text
   ~/.raven/workspace/skills/archify/SKILL.md
   ```

For a first-time terminal installation pinned to v2.13.0:

```bash
mkdir -p ~/.raven/workspace/skills
curl -fL https://raw.githubusercontent.com/tt-a1i/archify/v2.13.0/archify.zip \
  -o /tmp/archify-v2.13.0.zip
unzip /tmp/archify-v2.13.0.zip -d ~/.raven/workspace/skills
test -f ~/.raven/workspace/skills/archify/SKILL.md
```

The packaged Skill includes its renderer and zero-install validators. Do not run
`npm install` inside the installed Skill.

If an older `~/.raven/workspace/skills/archify` already exists, back it up before
extracting a new version. Do not merge two versions into the same directory.

## Prerequisites

- Raven with access to `~/.raven/workspace/skills`.
- Node.js available to Raven's shell environment.
- A local Git repository that Raven is allowed to read.
- An ordinary browser for opening the final self-contained HTML file.
- A clean output directory outside the target repository's source tree, unless
  the user intentionally wants the artifacts committed.

## Check the installation before the real task

Start a new Raven session after installation so that workspace Skills are
discovered again. Then ask Raven:

```text
请确认 archify Skill 是否可用。只检查 Skill，不要开始生成架构图。
```

If Raven can use the Skill, run the bundled diagnostics:

```bash
node ~/.raven/workspace/skills/archify/bin/archify.mjs doctor
node ~/.raven/workspace/skills/archify/bin/archify.mjs demo /tmp/archify-demo
```

Successful diagnostics prove that the packaged CLI can run. They do not yet
prove that Raven can analyze a specific repository accurately.

## Use case

### Goal

Turn a real repository into a high-level runtime architecture map that a new
engineer can use to answer four questions:

1. What are the 8–12 components that matter most at runtime?
2. What is the primary request or job path?
3. Where are the external dependencies and trust boundaries?
4. Which source files support the important architectural claims?

### Recommended first prompt

Open the target repository as Raven's working directory, then send:

```text
分析当前 Git 仓库的真实源码，然后使用 archify 创建一张高层运行时架构图。

要求：
1. 使用 architecture 类型和 showcase 质量档位。
2. 只保留 8–12 个核心组件，突出一条主要运行时路径。
3. 标出外部依赖、持久化组件和信任边界。
4. 辅助说明放进 cards，不要为了增加细节继续堆连线。
5. 基于当前 Git commit 添加源码证据；无法由源码证明的内容要明确标为推断。
6. 创建最多 3 个命名视图：主请求路径、数据持久化、失败与恢复。
7. 保留 typed JSON 源文件，并交付自包含 HTML。
8. 使用 Archify 的 validate 和 deliver 完成最终检查。
9. 最后返回：JSON 路径、HTML 路径、图表类型、校验回执、commit、
   人工视觉检查状态，以及所有仍未确认的推断。

不要修改目标仓库的产品代码，不要读取或展示密钥、环境变量值和用户数据。
```

### What Raven should do

1. Confirm the target repository and current Git commit.
2. Read the repository entry points, configuration, service boundaries, and
   primary runtime path.
3. Select `architecture`; do not mix five diagram types into one artifact.
4. Read only Archify's architecture schema, common schema, and one matching
   example before authoring the first candidate.
5. Write a fresh typed JSON candidate with stable IDs and repository-specific
   wording.
6. Run showcase validation after each candidate edit.
7. Make only focused repairs named by `diagnostics[]`; do not rewrite the whole
   diagram after a local geometry failure.
8. Use `deliver` for the final HTML and preserve the final JSON unchanged after
   the passing validation.
9. Perform a visual review when an image reader is available. Otherwise state
   that visual review was skipped.
10. Return the artifact paths and a truthful verification summary.

## Expected artifacts

```text
output/
├── repository-runtime.architecture.json
├── repository-runtime.html
└── validation-receipt.json
```

Optional reader exports can include:

- a PNG, JPEG, WebP, or SVG of the complete diagram;
- a 1200×630 share card for a README, release, or social post;
- a route or reach share card after selecting an authored path;
- a finite WebM presentation when motion was explicitly requested.

The checked-in example uses the same shape under [`example/`](example/):

```text
example/
├── raven-runtime.architecture.json
├── raven-runtime.html
├── raven-runtime.validation.json
├── raven-runtime.svg
└── raven-runtime-preview.png
```

## Acceptance criteria

- [ ] Archify v2.13.0 is present at the exact Raven Skill path.
- [ ] `doctor` succeeds.
- [ ] The JSON records the target repository and exact Git commit.
- [ ] The map contains 8–12 primary components and one obvious main path.
- [ ] External dependencies and trust boundaries are visible.
- [ ] Source-backed claims open revision-verified files and line ranges.
- [ ] Inferences are separated from verified repository evidence.
- [ ] Showcase validation reports all 9 artifact checks passing, with 0
      composition errors and 0 warnings.
- [ ] `deliver` exits successfully and returns the specification/artifact receipt.
- [ ] The delivered HTML opens, switches theme, searches nodes, restores named
      views, and explores a route without inventing topology.
- [ ] The handoff states whether visual review was completed or skipped.

## Focused refinement prompts

Use short follow-ups instead of regenerating everything:

```text
主路径不够明显。保留现有节点，只降低次要边的视觉权重。
```

```text
把鉴权边界移动到 API 与内部服务之间，不要改动其他分组。
```

```text
新增一个“失败与恢复”视图，只使用图中已经存在的节点和关系。
```

```text
检查数据库节点的源码证据。如果无法固定到当前 commit，就移除该证据标记，
并把说明改成“待确认”。
```

## Troubleshooting

### Raven cannot find the Skill

Check the exact path:

```bash
test -f ~/.raven/workspace/skills/archify/SKILL.md
```

Common causes:

- the ZIP was extracted as `skills/archify/archify/SKILL.md`;
- Raven was not restarted after installation;
- the Skill was installed under `.agents/skills`, which is a Codex/OpenCode
  location rather than Raven's workspace Skill location.

### `doctor` cannot run

Confirm that `node` is visible from the same environment Raven uses:

```bash
command -v node
node --version
```

The official package does not require `npm install`. A missing module normally
indicates an incomplete or mixed-version extraction rather than a dependency
installation step.

### Validation fails

Read the single JSON receipt, locate `diagnostics[]`, and change only the named
`subject` with one of its `supportedFixes`. Archify's Skill contract allows at
most two focused correction rounds when the error count does not improve.

### The map is too dense

Return to 8–12 primary nodes, remove low-value edges, keep one main path, and put
supporting detail in cards or named views.

## Limitations and safety

- Archify validates authored structure and artifact quality; it does not verify
  live infrastructure or production behavior.
- Source evidence is opt-in and only as accurate as the selected repository and
  pinned commit.
- Automatic Mermaid parsing, hosted sharing, general-purpose auto-layout, and a
  WYSIWYG editor are outside the official v2.13 scope.
- Generated HTML may expose repository names, local paths, component names, or
  source references. Review it before public sharing.
- Do not include secrets, raw environment files, customer data, or private
  repository source in the public use-case repository.

## Upstream resources

- [Archify repository](https://github.com/tt-a1i/archify)
- [Chinese README](https://github.com/tt-a1i/archify/blob/v2.13.0/README_ZH.md)
- [Archify Skill contract](https://github.com/tt-a1i/archify/blob/v2.13.0/archify/SKILL.md)
- [v2.13.0 release](https://github.com/tt-a1i/archify/releases/tag/v2.13.0)
- [Project site and Proof Lab](https://tt-a1i.github.io/archify/)

## Verification

- Status: `documented`
- Official version reviewed: Archify v2.13.0
- Documentation accessed: 2026-08-09
- Raven installation rerun: not yet
- Repository mapping rerun: not yet
- Maintainer: to be assigned

To promote this page to `verified`, add a clean Raven session transcript or
redacted run log, the pinned input commit, final typed JSON, delivered HTML,
validation receipt, and visual-review result.
