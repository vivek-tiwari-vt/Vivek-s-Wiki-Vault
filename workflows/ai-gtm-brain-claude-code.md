---
tags:
  - type/workflow
  - domain/ai
  - domain/business
  - domain/automation
  - topic/agent
  - topic/cli
  - workflow/automation
  - workflow/research
  - workflow/monitoring
  - status/reference
source_link: https://x.com/nifinet/status/2064397495036440907
context_link: https://x.com/i/article/2064389744268914689
source_type: x-article
kind: workflow
created: "2026-06-09"
updated: "2026-06-09"
canonical_name: AI GTM Brain with Claude Code
research_sources:
  - https://x.com/nifinet/status/2064397495036440907
  - https://x.com/i/article/2064389744268914689
  - https://code.claude.com/docs/en/overview.md
  - https://code.claude.com/docs/en/quickstart.md
last_verified_at: "2026-06-09"
unmapped_terms:
  - GTM brain
  - sense remember judge act learn
  - why now
  - dry run
  - intent signal
---

# AI GTM Brain with Claude Code

## Summary

Nicolas Finet's X Article **"How to Build an AI GTM Brain using Claude Code"** describes a command-line growth workflow that turns outbound from a larger send engine into a daily judgment loop. The system watches market movement, remembers each account, decides who is worth contacting and why now, drafts a trigger-specific opener, and learns from outcomes.

The source's main thesis: the valuable part of AI-assisted growth is not sending more messages. It is making the daily judgment around **which company moved**, **why that movement matters this week**, and **what message proves you noticed**. Sending is the easy part once that judgment loop exists.

## Source Context

- Triggering source: Nicolas Finet / `@nifinet` shared an X status linking to an X Article.
- Article title: **"How to Build an AI GTM Brain using Claude Code"**.
- Article ID: `2064389744268914689`; tweet/status ID: `2064397495036440907`.
- Source framing: a GTM brain is a command-line agent loop with five organs — sense, remember, judge, act, and learn — rather than a mail-merge prompt over a larger list.
- Public article metadata exposed the main article body and seven embedded diagrams plus a cover image.
- The article says it includes prompts pasted into Claude Code, but public metadata did not expose the full prompt bodies; the embedded images inspected here were diagrams, not readable full prompt templates.
- Social metrics and source-reported business claims are treated as source-reported, not independently reproduced.

## What It Is

An AI GTM brain is a go-to-market automation loop run from the command line on a schedule. It is meant to help a growth or sales team decide which accounts deserve outreach now, not simply automate bulk sending.

The five-part loop is:

1. **Sense**: watch the market for accounts that just moved.
2. **Remember**: maintain account history across signals, touches, messages, replies, and outcomes.
3. **Judge**: decide whether an account is worth a message right now, why now, and which play to run.
4. **Act**: write or prepare an opener grounded in the real trigger.
5. **Learn**: feed outcomes back into memory so signal weights and copy choices improve.

This is a domain-specific version of [[Agentic Coding Loops]]: a scheduled loop, persistent state, judgment step, action step, feedback step, and human review boundary.

## What It Does

The workflow converts raw market movement into a ranked shortlist of accounts and drafted outreach. It is designed to:

- cluster job, social, company, and funding signals by account;
- avoid treating every single signal as equally important;
- keep one memory record per company;
- use account history to avoid repeated or tone-deaf outreach;
- score whether the company is worth contacting now;
- produce a `why_now` reason that can be used directly in outreach;
- write a draft that quotes the trigger rather than using generic personalization;
- default delivery to dry-run so humans can inspect messages before sending;
- log replies, meetings, no-replies, and bounces as outcomes;
- re-weight future signal buckets and plays based on outcomes.

The promised daily output is a ranked shortlist in Slack or a similar review surface: account, reason, draft, and approve/edit/kill decision. Human review remains the final boundary before anything live is sent.

## How It Works

The source article presents six build steps:

1. **The contract**: define the boundary before code. Use only two shapes: `fetch() -> Signal` for incoming market movement and `send(draft) -> id` for outgoing delivery. Vendor-specific tools stay behind adapters.
2. **Sense the market**: collect movement signals from job, social, company, and funding events. Cluster at the account level before ranking.
3. **Remember every account**: keep full account history, including prior signals, touches, messages, replies, outcomes, and notes.
4. **Judge who and why now**: have Claude read the new signal, account memory, and ideal-customer definition, then decide whether to message, why, and what play to run.
5. **Act on the trigger**: draft only after judgment; quote the real trigger in the opener. Delivery defaults to dry-run, with `--live` as an explicit decision.
6. **Learn from outcomes**: feed reply and meeting data back into memory; keep signal buckets and copy that work, cut what does not.

A simplified state flow:

```text
market signals -> signal adapters -> account memory -> judge -> draft -> dry-run review -> send -> outcomes -> memory/reweighting
```

Claude Code is used as the builder/operator environment: a command-line agent that can create files, edit code, run commands, and integrate with a development workflow. Official Claude Code docs describe it as an agentic coding tool available in the terminal, IDE, desktop app, and browser.

## Diagram Notes

The embedded diagrams reinforce the article's architecture:

- **Cover**: "The GTM Brain" with the sequence build guide: sense, remember, judge, act, learn.
- **Judgment is the moat**: new signal, memory, and ICP flow into a judge; outputs are score, why-now, and play.
- **It marks its own homework**: outcomes re-weight buckets such as funding, job, company, and social while keeping the prompt stable.
- **Start from movement, not a list**: job, social, company, and funding events cluster by account before becoming a hot account.
- **The brain stays vendor-free**: signal adapters and delivery adapters surround the core five-part brain.
- **The organ everyone skips**: one account record stores signals, touches, outcomes, and notes; the judge reads it every run.
- **Five organs, one loop**: sense → remember → judge → act → learn.
- **Quote the trigger**: contrasts a generic send-button template with a trigger-specific opener.

## Use Cases

Good candidates:

- outbound prospecting where there are real account-level intent signals;
- account-based marketing with many signals but limited human prioritization time;
- daily or weekly sales-review workflows that need ranked account shortlists;
- founder-led or small-team sales where judgment matters more than message volume;
- growth teams that already have reply/meeting outcome data to close the loop;
- teams that can run outreach in dry-run mode before allowing live sends.

Poor candidates:

- generic bulk sending with no account movement or trigger;
- teams without memory of prior touches and outcomes;
- markets where reliable signals are unavailable;
- compliance-sensitive outreach where message approval cannot be delegated;
- teams that cannot review drafts before live delivery;
- workflows where reply and meeting outcomes never get captured.

## Why It Matters

The note is a business-side analogue to [[Agentic Coding Loops]] and [[Hermes Standing Agent Prompts]]. The same loop primitives show up: recurring trigger, persistent state, judgment, action, feedback, and safety gates. The difference is the domain: GTM work instead of code maintenance.

The source's sharpest product insight is that **memory is not optional**. Without account memory, the agent only sees the latest signal and may repeat stale outreach. With memory, the third funding event, a new RevOps hire, or a competitor mention can be interpreted in the context of previous touches, silence, replies, and outcomes.

The second useful idea is adapter discipline. If the core loop names vendors directly, a tooling change rewrites the system. If the core loop talks only to signal and delivery contracts, switching intent providers or email tools becomes an adapter job.

## Risks and Guardrails

- **Spam at scale**: a GTM loop can produce confident irrelevant outreach quickly. Keep dry-run as the default and require human approval before live sends.
- **Bad memory causes bad judgment**: stale or incomplete account history can make the judge repeat old plays or misread a signal.
- **Vendor lock-in**: calling a specific intent or email vendor inside core logic welds the workflow to that vendor.
- **Signal noise**: sensing everything without clustering or ranking pushes noise downstream and makes the judge less reliable.
- **Outcome blindness**: logging sends without replies, meetings, bounces, and no-replies prevents learning.
- **Privacy/compliance**: account-level history and outreach data may include regulated personal data; access controls and retention rules matter.
- **Over-automation**: live send should be an explicit flag or approval, not the default behavior.

## Implementation Pattern

A minimal implementation should start with memory and judgment before live sending:

1. Define `Signal` and `Draft` contracts.
2. Build fake/stub signal data.
3. Create account memory storage.
4. Run the judge against stub data until it produces useful `score`, `why_now`, and `play` fields.
5. Generate drafts in dry-run only.
6. Add one real signal source.
7. Add outcome ingestion.
8. Schedule the loop once dry-run output earns trust.

The source suggests starting with only memory plus judge, running it dry for a week, then wiring in one real signal source. That is the sensible rollback path: if the judge is poor, no live delivery has happened.

## Related Notes

- [[Agentic Coding Loops]] for the general scheduled-agent-loop pattern.
- [[Hermes Standing Agent Prompts]] for persistent scheduled-agent prompts with triggers, budgets, and escalation rules.
- [[Claude Dynamic Workflows]] for script-orchestrated multi-agent work when the loop grows beyond one command.
- [[Claude Code]] for the underlying agentic coding assistant surface referenced by the source.

## Sources

- [Nicolas Finet X status](https://x.com/nifinet/status/2064397495036440907) — source post linking to the X Article.
- [X Article: How to Build an AI GTM Brain using Claude Code](https://x.com/i/article/2064389744268914689) — source article and workflow description.
- [Claude Code overview docs](https://code.claude.com/docs/en/overview.md) — official description of Claude Code as an agentic coding tool.
- [Claude Code quickstart docs](https://code.claude.com/docs/en/quickstart.md) — official terminal CLI getting-started context.
