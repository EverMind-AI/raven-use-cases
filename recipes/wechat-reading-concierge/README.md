---
title: WeChat Reading Concierge
type: recipe
status: documented
author: "Community contributor (name pending)"
source: "Community submission (unpublished)"
difficulty: beginner
raven_version: "Unrecorded; commands checked against v0.1.10"
last_verified: "2026-08-12 (community-provided desktop captures reviewed; maintainer rerun pending)"
tags: [wechat, reading, memory, web, cron]
---

# WeChat Reading Concierge

[简体中文](README.zh-CN.md)

## Problem

Articles forwarded through WeChat are easy to lose in Favorites or chat
history. Titles are hard to remember, folders require manual maintenance, and a
saved link carries little context about why it mattered.

This recipe turns one Raven WeChat conversation into a lightweight reading
inbox. Raven can read a public link, summarize it, remember the link together
with the user's note and formatting preferences, and retrieve it later by idea
rather than exact title. An optional scheduled job can prepare a weekly reading
digest.

## Included evidence

The images below are community-provided captures from an actual desktop chat
run. They demonstrate link capture, summary refinement, and later recall. They
do not by themselves verify a long time gap, a different agent, or successful
scheduled delivery.

### Save a link with context

![A public article link sent to Raven with a note to read it next week](example/01-forward.png)

### Refine the preferred summary length

![Raven shortens the article summary after the user asks for one or two lines](example/02-summary-preference.png)

### Recall the article by topic

![Raven recalls the saved article and the user's reading note from a topic-level question](example/03-recall.png)

## Prerequisites

- Raven installed with an LLM provider configured.
- EverOS selected as Raven's memory backend.
- The `weixin` channel enabled and connected to the intended WeChat account.
- Raven's gateway process kept running while the WeChat assistant is in use.
- Network access to the public articles Raven is asked to read.
- A Serper key at `tools.web.search.apiKey` only when the workflow also needs
  live web search; fetching a supplied public URL is a separate operation.

## Inputs

- A public article URL.
- An optional note explaining why the article was saved.
- Optional feedback about summary length, tone, language, or topics to avoid.
- For the weekly digest: a weekday, time, timezone, and maximum item count.

Do not submit confidential documents, private links, authentication tokens, or
sensitive personal information unless the user has reviewed the storage and
privacy implications of the configured memory backend.

## Setup

Install Raven using the command from the official repository:

```bash
curl -fsSL https://raven.evermind.ai/install.sh | bash
```

Run onboarding, select EverOS as the memory backend, and enable WeChat:

```bash
raven onboard
```

Check the resulting setup before starting the real workflow:

```bash
raven doctor
raven plugins
raven channels list
```

`raven plugins` should show the EverOS memory plugin and its active backend.
`raven channels list` should show `weixin` as enabled.

Start the gateway and keep the process running:

```bash
raven gateway
```

## Workflow

### 1. Save an article with a reason

Send a public URL to the Raven contact in WeChat. Add one sentence of context;
it makes later retrieval more useful than a bare bookmark.

```text
https://example.com/article

Save this for the research project I plan to start next month.
```

Check that Raven identifies the article and preserves both the URL and the
reason for saving it. If the page is unavailable, Raven should say that it saved
the link and note without claiming to have read the body.

### 2. Ask for a summary

```text
Summarize the article in three bullets and include its main conclusion.
```

The response should be based on the fetched article, not only its title or URL.
For a verification run, ask one factual question whose answer appears in the
article body.

### 3. Teach a presentation preference

Give ordinary feedback instead of editing a settings file:

```text
Keep future article summaries to one or two lines unless I ask for detail.
```

Then send another article and check whether Raven applies the preference. One
same-session correction demonstrates instruction following; a new-session test
is needed before claiming durable preference memory.

### 4. Recall by idea in a fresh session

After the earlier conversation has ended, start a new Raven session and ask a
topic-level question without repeating the article title:

```text
What was that article I saved about agent memory?
```

A successful result should return the right article, its URL, and the user's
original reason for saving it. Record the two session identifiers and dates if
this evidence will be used to mark the recipe `verified`.

### 5. Add an optional weekly digest

The most reliable user flow is to ask Raven inside the same WeChat conversation
that should receive the digest:

```text
Every Saturday at 10:00 Asia/Shanghai time, review the articles I saved this
week plus older ones I have not confirmed reading. Group them by theme,
summarize them in my preferred style, flag useful connections or contradictions,
and include no more than 10 items.
```

Raven should confirm the schedule and destination. Inspect the saved job from a
terminal:

```bash
raven cron list
raven cron get <job-id>
```

For scripting, the equivalent advanced command is:

```bash
raven cron add \
  --name "reading-digest" \
  --cron "0 10 * * 6" \
  --tz "Asia/Shanghai" \
  --message "Search memory for articles I saved this week plus older ones I have not confirmed reading. Group them by theme, summarize each in my preferred style, flag useful connections or contradictions, and include no more than 10 items." \
  --yes
```

When `--channel` and `--to` are omitted, Raven routes this CLI-created job at
trigger time using `cron.forward_channels` and the most recent channel session.
Before relying on it, check:

```bash
raven cron config get
raven channels list
```

Keep the gateway running through the scheduled time and verify the first real
delivery before treating the automation as complete.

## Expected output

The basic workflow should produce:

1. A saved article URL and user note.
2. A summary grounded in the article body when the page can be fetched.
3. A summary-format preference that can be tested on another article.
4. Topic-level recall in a fresh session, including the original link.

The optional automation should produce a WeChat digest containing at most ten
items, grouped by theme, using the tested summary style. A digest screenshot has
not yet been included in this recipe.

## Acceptance criteria

- [ ] `raven doctor` reports no blocking configuration errors.
- [ ] `raven plugins` confirms the intended EverOS memory backend is active.
- [ ] `raven channels list` shows `weixin` enabled.
- [ ] Raven distinguishes a successfully fetched article from a saved-only URL.
- [ ] A factual question confirms that the article body was read.
- [ ] Raven recalls the correct link and saving note in a fresh session.
- [ ] A second article tests whether the summary preference persists.
- [ ] The first weekly digest reaches the intended WeChat conversation.
- [ ] `raven cron get <job-id>` shows the expected schedule and delivery state.
- [ ] Public screenshots are real, authorized, and free of sensitive data.

## Limitations

- Pages behind logins, paywalls, anti-bot checks, or heavy client-side rendering
  may not be readable. Raven must not present title-only inference as a fetched
  summary.
- Web content can change or disappear. Keep the original URL and record the
  access date when factual fidelity matters.
- Cross-session recall depends on the same configured memory identity and a
  successful memory write. It is not guaranteed merely because Raven replied in
  the original chat.
- A preference observed in one conversation is not proof that a reusable
  procedure or Skill has been created.
- Scheduled delivery requires a running Raven gateway, a valid destination, and
  correct timezone and channel routing.
- Article copyright remains with its source. Store summaries and links rather
  than republishing full article text.

## Attribution

- Author: Community contributor; public name or handle pending.
- Original source: Unpublished community submission reviewed on 2026-08-12.
- Screenshots: Community-provided actual desktop chat captures; redistribution
  permission must be confirmed before publication.

## Upstream resources

- [Raven repository](https://github.com/EverMind-AI/Raven)
- [EverOS repository](https://github.com/EverMind-AI/EverOS)
- [Raven documentation](https://raven.evermind.ai/)

## Verification

- Status: `documented`
- Raven version in original run: not recorded
- Commands reviewed against: Raven v0.1.10
- Evidence reviewed: link capture, summary refinement, topic-level recall
- Not yet independently verified: fresh-session recall, preference persistence,
  weekly digest delivery, and cross-agent access
- Last reviewed: 2026-08-12
- Maintainer: unassigned

To promote this recipe to `verified`, rerun it on a clean Raven setup, record the
Raven version and two session identifiers, and add one successful weekly digest
capture with its cron receipt.
