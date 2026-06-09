---
tags:
  - type/workflow
  - domain/ai
  - domain/automation
  - domain/software-engineering
  - topic/agent
  - topic/cli
  - workflow/automation
  - workflow/monitoring
  - workflow/documentation
  - status/reference
source_link: https://x.com/Mnilax/status/2063697740526399833
context_link: https://x.com/i/article/2063676886031495171
source_type: x-article
kind: workflow
created: "2026-06-09"
updated: "2026-06-09"
canonical_name: Hermes Standing Agent Prompts
research_sources:
  - https://x.com/Mnilax/status/2063697740526399833
  - https://x.com/i/article/2063676886031495171
  - https://github.com/NousResearch/hermes-agent
  - https://raw.githubusercontent.com/NousResearch/hermes-agent/main/README.md
  - https://raw.githubusercontent.com/NousResearch/hermes-agent/main/website/docs/user-guide/features/cron.md
  - https://raw.githubusercontent.com/NousResearch/hermes-agent/main/website/docs/user-guide/features/skills.md
  - https://raw.githubusercontent.com/NousResearch/hermes-agent/main/website/docs/user-guide/messaging/index.md
  - https://raw.githubusercontent.com/NousResearch/hermes-agent/main/website/docs/user-guide/configuration.md
last_verified_at: "2026-06-09"
unmapped_terms:
  - standing agent
  - serverless backend
  - escalation rule
  - $5 VPS
---

# Hermes Standing Agent Prompts

## Summary

The source article, **"17 prompts that make Hermes run while you sleep"**, frames Hermes Agent as a persistent runtime that becomes useful only after the user defines standing jobs: scheduled briefs, repo watches, inbox triage, research digests, on-call diagnosis, and repeatable skills. Its central mental model is that a prompt to a chat window is a question, while a prompt to a persistent agent is a **job description** with three parts: a trigger, a body, and an escalation rule.

Official Hermes material supports the underlying capabilities: Hermes has scheduled tasks through cron jobs, a messaging gateway across many platforms, persistent memory/skills, provider configuration, and local/remote tool execution. The article's personal numbers and runtime anecdotes are source-reported and should be treated as examples, not benchmarks.

## Source Context

- Triggering source: Mnimiy / `@Mnilax` shared an X status linking to an X Article.
- Article title: **"17 prompts that make Hermes run while you sleep (copy-paste inside)"**.
- Article ID: `2063676886031495171`; tweet/status ID: `2063697740526399833`.
- Author framing: Hermes is a runtime, not a workflow; the workflows are the standing prompts you configure on day one.
- Source-reported setup: five weeks using Hermes on a low-cost VPS with Claude underneath.
- Source-reported metrics: public social metrics and claimed time savings were captured from public metadata, but not independently reproduced.
- Extraction note: article text and six images were available. The public article blocks exposed recipe headings and explanations; the truly copy-paste prompt bodies were not all present as plain text in the public metadata. One visible image did show the prompt structure example: `every weekday at 7am, pull my GitHub notifications and summarize them, only ping me if something is blocking a deadline`.

## What It Is

Hermes Standing Agent Prompts are recurring, persistent-agent job descriptions for Hermes Agent. They are not one-off chat prompts. Each prompt defines:

1. **Trigger**: schedule or event, such as every weekday at 7am, hourly, Friday evening, on new CI failure, or on incoming message.
2. **Body**: what Hermes should read, compare, summarize, diagnose, draft, or watch.
3. **Escalation rule**: when Hermes should bother the user, stay silent, ask for approval, or deliver a report.
4. **Budget and scope**: token limits, source limits, repository limits, timeframe, and maximum retries.
5. **Delivery path**: Telegram, Slack, email, local file, or another configured platform.
6. **Safety boundary**: no writes, draft-only, human approval required, sandboxed shell, or read-only repo access.

The source article argues that this framing turns Hermes from a blank install into standing infrastructure for unattended work.

## What It Does

The article lists seventeen categories of setup prompts and configuration moves:

1. **7am brief**: summarize GitHub notifications or other morning triage before the user starts work.
2. **Repo watch**: monitor CI or regressions and speak only when a meaningful failure appears.
3. **Cross-channel inbox triage**: monitor Telegram, Discord, Slack, WhatsApp, Signal, and email from one process; escalate only deadlines, waiting people, or money-related items.
4. **Friday research digest**: deduplicate repeated feed items and produce a short weekly digest.
5. **Repo map**: inspect an unfamiliar repository and produce a fast working map.
6. **Overnight long task**: make reasonable assumptions, continue without waiting, and return assumptions explicitly.
7. **Competitor changelog watch**: monitor competitor release notes rather than social feeds.
8. **Nightly code review**: scan for risky changes such as leaked secrets, debug logs, or obvious regressions.
9. **Stand-up draft**: reconstruct yesterday's work and prepare a concise update.
10. **Name radar**: surface mentions of the user/product/brand from noisy public channels.
11. **Talk summarizer**: turn a long talk into a short bullet summary with useful timestamps.
12. **Error explainer**: analyze a stack trace, identify the likely failing line, and propose a small fix.
13. **Inbox-zero reply drafts**: draft routine replies while requiring human approval before sending.
14. **On-call pre-diagnosis**: investigate logs before paging the user with a hypothesis.
15. **Model selection**: use a model with enough context and tool reliability for multi-step jobs.
16. **Idle-cost backend**: prefer an execution backend that does not run up standing compute cost while idle.
17. **Skill creation**: turn a successful run into a durable `SKILL.md` so the user does not retype the same operating procedure.

## How It Works

A practical Hermes standing prompt should be written like this:

```text
Every <schedule/event>, inspect <sources>. Do <specific analysis>. Only notify me when <escalation condition>. Otherwise stay silent or write to <log/file>. Use at most <budget>. If uncertain, label assumptions and ask before destructive actions.
```

Official Hermes docs map this to concrete subsystems:

- **Cron / scheduled tasks**: Hermes can create one-shot or recurring jobs, attach skills, deliver results to the origin chat, local files, or platform targets, and run no-agent script-only watchdogs.
- **Messaging gateway**: a single gateway process connects Hermes to Telegram, Discord, Slack, WhatsApp, Signal, SMS, email, Matrix, Home Assistant, Mattermost, Microsoft Teams, LINE, ntfy, and other surfaces depending on configuration.
- **Skills**: durable knowledge documents under `~/.hermes/skills/` can encode recurring procedures, templates, scripts, verification rules, and source-specific playbooks.
- **Configuration**: `~/.hermes/config.yaml`, `~/.hermes/.env`, `SOUL.md`, memories, skills, cron jobs, sessions, and logs live under the Hermes home directory.
- **Model/provider routing**: Hermes can be configured for OpenRouter, Anthropic, OpenAI/Codex, Nous Portal, local or custom providers, and other provider integrations.
- **Tool execution**: standing jobs can use terminal, file, browser/web, messaging, and other tools according to enabled toolsets and permissions.

## Use Cases

Good candidates for Hermes standing prompts:

- Morning or weekly operational briefings.
- CI, deploy, and regression monitoring where silence matters more than constant updates.
- Inbox triage that must span multiple platforms.
- Competitive-intelligence and changelog monitoring.
- Periodic research digests with deduplication against prior reports.
- Nightly low-risk code review and secret/debug-log scans.
- On-call diagnosis that gathers logs and hypotheses before waking a human.
- Report, video, podcast, or meeting summarization when new content appears.
- Routine reply drafting with explicit human approval before send.
- Any repeatable workflow that should graduate into a Hermes skill.

## Why It Matters

The durable insight is not the exact prompt list; it is the shift from **conversation** to **standing work**. A persistent agent has memory, clocks, tools, and delivery channels. That means the user should define policies for when it acts, when it remains silent, and when it escalates.

The most useful prompt is often not the most verbose one. It is the one with a crisp trigger, bounded input set, narrow output shape, token/spend cap, and explicit escalation condition. Without that, a scheduled agent produces noise, spends too much, or stalls waiting for clarification.

## Risks and Guardrails

- **Over-notification**: vague prompts create firehoses. Every scheduled job needs an explicit "only notify me when..." clause.
- **Cost creep**: persistent hourly jobs can spend more than expected. Add token/source/time budgets and review spend during the first week.
- **Model mismatch**: source claims small local models dropped tool calls on multi-step jobs. Treat model choice as part of reliability, not only cost.
- **Privilege creep**: a standing agent that remembers and runs shell commands needs stricter boundaries than a disposable chatbot.
- **Self-hosting burden**: updates, uptime, logs, permissions, backups, and gateway delivery are now operator responsibilities.
- **Unsafe writes**: inbox replies, code changes, deploy actions, and destructive shell commands should default to draft/approval modes unless intentionally automated.
- **Hype distortion**: source article warns that star counts and deployment guides can be noisy; judge the setup by operational usefulness.
- **Extraction gap**: the source title promises copy-paste prompts, but public metadata extraction did not expose every prompt body as text; preserve the prompt categories and mental model rather than inventing missing prompt text.

## Implementation Checklist

For a new Hermes standing job:

1. Pick one recurring pain point, not all seventeen at once.
2. Define the trigger: schedule, event, or manual command.
3. Define the input set: repos, channels, feeds, logs, docs, or files.
4. Define the output: brief, table, patch suggestion, draft reply, ticket, or silence.
5. Define escalation: when to notify immediately, summarize later, ask approval, or do nothing.
6. Define budget: max tokens, max sources, max runtime, max retries, and delivery cadence.
7. Add safety: read-only first, no destructive actions, require approval for sends/deploys/deletes.
8. Run a short trial and inspect output quality and spend.
9. Convert the successful procedure into a skill if it will recur.
10. Keep a rollback path: pause/remove the cron job and revert any file changes or approvals.

## Related Notes

- [[Claude Code]] for single-project agentic coding work that is session-shaped.
- [[Agentic Coding Loops]] for the broader loop-control model behind autonomous work.
- [[Claude Dynamic Workflows]] for script-orchestrated multi-agent workflows.
- [[Best Practices for Claude Code]] for verification, context, and autonomy practices.
- [[Claude Code Skills Lessons]] for turning repeatable workflows into durable skills.

## Official Project Metadata

- Project: `NousResearch/hermes-agent`
- Description from GitHub API: **"The agent that grows with you"**
- Primary language: Python
- License: MIT
- Homepage: `https://hermes-agent.nousresearch.com`
- GitHub metadata checked 2026-06-09: public repository, default branch `main`, high star/fork counts. Treat counts as time-sensitive.

## Sources

- [Mnimiy X status](https://x.com/Mnilax/status/2063697740526399833) — source post linking to the X Article.
- [X Article: 17 prompts that make Hermes run while you sleep](https://x.com/i/article/2063676886031495171) — source article and prompt categories.
- [Hermes Agent GitHub repository](https://github.com/NousResearch/hermes-agent) — official project repository and metadata.
- [Hermes Agent README](https://raw.githubusercontent.com/NousResearch/hermes-agent/main/README.md) — official project description and feature overview.
- [Hermes Scheduled Tasks / Cron docs](https://raw.githubusercontent.com/NousResearch/hermes-agent/main/website/docs/user-guide/features/cron.md) — official scheduled-job behavior and delivery modes.
- [Hermes Skills System docs](https://raw.githubusercontent.com/NousResearch/hermes-agent/main/website/docs/user-guide/features/skills.md) — official skill mechanics and persistence model.
- [Hermes Messaging Gateway docs](https://raw.githubusercontent.com/NousResearch/hermes-agent/main/website/docs/user-guide/messaging/index.md) — official platform/gateway overview.
- [Hermes Configuration docs](https://raw.githubusercontent.com/NousResearch/hermes-agent/main/website/docs/user-guide/configuration.md) — official paths and configuration model.
