---
tags:
  - type/workflow
  - domain/ai
  - domain/software-engineering
  - domain/automation
  - topic/agent
  - topic/llm
  - topic/cli
  - workflow/automation
  - workflow/monitoring
  - status/reference
source_link: https://x.com/mvanhorn/status/2063865685558903149
context_link: https://x.com/i/article/2063850827694096385
source_type: x-article
kind: workflow
created: "2026-06-09"
updated: "2026-06-09"
canonical_name: Agentic Coding Loops
research_sources:
  - https://x.com/mvanhorn/status/2063865685558903149
  - https://x.com/i/article/2063850827694096385
  - https://x.com/addyosmani/status/2064127981161959567
  - https://x.com/i/article/2064122477731852288
  - https://x.com/0xCodez/status/2064374643729773029
  - https://x.com/i/article/2064357550225510400
  - https://code.claude.com/docs/en/goal.md
  - https://code.claude.com/docs/en/scheduled-tasks.md
  - https://code.claude.com/docs/en/workflows.md
  - https://developers.openai.com/codex/app/automations.md
  - https://developers.openai.com/codex/app/worktrees.md
  - https://developers.openai.com/codex/skills.md
  - https://developers.openai.com/codex/subagents.md
  - https://react-lm.github.io/
  - https://github.com/Significant-Gravitas/AutoGPT
  - https://ghuntley.com/ralph/
  - https://github.com/gastownhall/gastown
last_verified_at: "2026-06-09"
unmapped_terms:
  - ralph loop
  - /goal
  - /loop
  - Gas Town
  - Loop Engineering
  - Codex Automations
  - 14-step roadmap
  - cognitive surrender
  - comprehension debt
---

# Agentic Coding Loops

## Summary

Agentic coding loops are repeatable control structures that prompt coding agents, inspect the results, decide whether the work is complete, and either stop or run another iteration. The important shift is that the human is no longer typing each prompt inside the loop; the human designs the loop, the feedback gates, the stop conditions, and the reusable skills the loop can call.

Matt Van Horn's X Article, **"WTF Is a Loop? Peter Steinberger vs. Boris Cherny,"** frames the current discourse around Peter Steinberger's line: "you shouldn't be prompting coding agents anymore; you should be designing loops that prompt your agents." The article argues that the term "loop" spans a spectrum: ReAct-style tool loops, AutoGPT-style autonomous goals, simple repeated prompt loops such as Ralph, productized `/goal` and `/loop` commands, and newer multi-agent orchestration loops with scheduling, durability, and verification.

Addy Osmani's X Article, **"Loop Engineering,"** sharpens that into a cross-tool architecture: instead of prompting a coding agent manually, the engineer designs the system that discovers work, schedules it, isolates parallel work, applies project skills, connects to real tools, delegates to subagents, verifies results, and remembers state outside the conversation.

Codez's X Article, **"Loop engineering: the 14-step roadmap from prompter to loop designer,"** turns the same idea into an adoption checklist: first test whether a loop is justified, then assemble the five primitives, then build the smallest safe loop with one automation, one skill, one state file, and one objective gate.

## Source Context

- Triggering source: Matt Van Horn's X status linking to an X Article.
- Article title: **"WTF Is a Loop? Peter Steinberger vs. Boris Cherny"**.
- Article ID: `2063850827694096385`; tweet/status ID: `2063865685558903149`.
- Source framing: the term "loop" is being used loosely in AI-coding discourse, but the durable idea is a program or scheduled process that prompts agents, reads results, verifies progress, and repeats with guardrails.
- Additional source: Addy Osmani / `@addyosmani` shared the X Article **"Loop Engineering."**
- Addy article ID: `2064122477731852288`; tweet/status ID: `2064127981161959567`.
- Addy framing: loop engineering is replacing yourself as the person who prompts the agent; the engineer designs a recursive goal system that prompts, checks, records state, and continues.
- Additional source: Codez / `@0xCodez` shared the X Article **"Loop engineering: the 14-step roadmap from prompter to loop designer."**
- Codez article ID: `2064357550225510400`; tweet/status ID: `2064374643729773029`.
- Codez framing: the practical roadmap has three tiers — decide whether a loop is warranted, learn the five building blocks, then build the minimum viable loop without removing human engineering judgment.
- Metrics in the source posts/articles are source-reported and not independently reproduced here.
- Article images from the Codez post included diagrams for loop structure, tool-primitives mapping, evaluator-optimizer flow, minimum viable loop, loop-need decision gate, connectors UI, agent loop cycle, prompt-loop comparison, and `/goal` closing the loop.

## What It Is

An agentic coding loop is a workflow primitive for autonomous or semi-autonomous coding work. At minimum, it contains:

1. **A task or intent**: what the coding agent should pursue.
2. **A prompt generator or fixed prompt**: what the agent receives each iteration.
3. **A work executor**: the coding agent, CLI, background worker, or subagent.
4. **A feedback reader**: tests, CI, review comments, diff checks, logs, issue queues, or source status.
5. **A completion evaluator**: deterministic checks, a validator model, a human approval gate, or a combination.
6. **A next-step policy**: stop, retry, refine, route, escalate, or schedule another pass.
7. **Guardrails**: iteration caps, spend caps, no-progress detection, permission boundaries, and rollback paths.

In plain terms: a loop is not merely "asking harder." It is a small operating system around the agent's work.

## What It Does

Agentic coding loops can:

- keep a session working across turns until a verifiable condition is met;
- poll CI, deployments, reviews, issue queues, or pull requests;
- rerun an agent when tests fail or review comments arrive;
- keep a branch healthy by fixing build failures and merge conflicts;
- apply the same review or migration process across many files or repositories;
- invoke reusable skills or saved commands rather than re-deriving the process each time;
- supervise other agents, sessions, or workflows;
- preserve state through git-backed logs, task ledgers, or durable orchestration state.

Official Claude Code docs now expose several loop-like primitives:

- `/goal`: sets a completion condition; Claude keeps working turn after turn until a small fast model confirms the condition is met.
- `/loop`: reruns a prompt on an interval or at a Claude-chosen cadence; useful for polling PRs, builds, deployments, and maintenance tasks during a session.
- Dynamic workflows: JavaScript-orchestrated subagent runs for larger multi-stage work where script variables hold intermediate state.

Official OpenAI Codex docs now expose comparable pieces:

- Automations: recurring background tasks that add findings to a triage inbox or archive themselves when there is nothing to report.
- Worktrees: independent Git worktrees for parallel tasks so background work does not interfere with the local checkout.
- Skills: reusable workflow packages with `SKILL.md`, optional scripts, and references.
- Subagents: explicitly requested parallel specialized agents whose results are consolidated into one response.

## How It Works

A robust coding loop usually follows this shape:

```text
while budget_remaining and iteration_count < max_iterations:
    observe current repo / issue / PR / CI state
    prompt the agent with the current goal and fresh evidence
    let the agent edit, run tools, or produce a result
    verify with tests, lint, CI, review, or a judge model
    if completion condition is satisfied:
        stop and report success
    if no progress is detected:
        stop, escalate, or change strategy
    otherwise:
        schedule or run the next iteration
```

The source article distinguishes several layers of this idea:

- **ReAct-style loop**: the model reasons, calls a tool, observes the result, and repeats.
- **AutoGPT-style loop**: a goal-seeking agent prompts itself repeatedly, often with weak stopping behavior.
- **Ralph-style loop**: a simple shell loop repeatedly prompts the agent while resetting or anchoring context.
- **Productized goal loops**: commands like `/goal` make the completion condition explicit and use a small evaluator to decide whether to continue.
- **Scheduled loops**: commands like `/loop` run prompts on a fixed or dynamically chosen cadence.
- **Multi-agent orchestration loops**: systems such as dynamic workflows or Gas Town coordinate many agents, preserve state, and supervise long-running work.
- **Loop engineering systems**: automations discover and triage work, worktrees isolate parallel execution, skills preserve project intent, connectors touch external systems, subagents split maker/checker roles, and a state file or board remembers what happened.

The practical engineering challenge is not the existence of a loop; `while true` is old. The challenge is making the loop halt, verify itself, preserve useful state, avoid compounding bad commits, and operate within a budget.

## Loop Engineering Components

Addy's five-piece framing maps well onto implementation checklists:

1. **Automations / heartbeat**: scheduled or event-triggered jobs discover work and decide whether anything deserves attention.
2. **Worktrees / isolation**: parallel agents need separate branches/checkouts so they do not overwrite one another.
3. **Skills / project memory**: reusable instructions encode conventions, build steps, review rubrics, and past incidents so each run does not re-derive them.
4. **Plugins and connectors / real tools**: MCP connectors, issue trackers, databases, staging APIs, Slack, Linear, and PR systems let loops act in the actual environment.
5. **Subagents / maker-checker split**: one agent explores or implements; another verifies against the spec, tests, skills, or security rubric.
6. **External state / memory**: a markdown file, board, issue tracker, or durable state store holds what has been tried, what passed, and what is next.

The most useful loop shape in the article is a morning automation that reads CI failures, issues, and recent commits; writes findings to a markdown file or board; opens isolated worktrees for worthwhile fixes; runs maker and verifier subagents; updates tickets/PRs through connectors; and leaves unresolved work in a triage inbox.

## Adoption Checklist

The Codez roadmap adds a useful constraint: **most developers should not build loops until the task passes a hard readiness test**. A loop is worth considering only when:

1. **The task repeats**: setup cost must amortize across many runs, ideally at least weekly.
2. **Verification is automated**: a test suite, type checker, linter, build, benchmark, or other gate can reject bad output without the human reading every diff.
3. **The token/compute budget can absorb waste**: loops re-read context, retry, explore, and fan out, so they cost more than one good prompt.
4. **The agent has senior-engineer tools**: logs, a reproduction environment, runtime access, and the ability to run the code it changes.
5. **The loop has a hard stop**: iteration count, wall-clock limit, token/spend budget, or an objective completion condition.
6. **A human reviews irreversible actions**: merges, deploys, dependency upgrades, auth, payments, and architecture changes require approval.

The minimum viable loop is deliberately small:

- **One automation**: a scheduled or event-triggered run.
- **One skill**: project context, commands, rules, and gotchas the agent rereads every run.
- **One state file**: markdown, JSON, issue tracker, or board that records what happened and what is next.
- **One objective gate**: a deterministic check that can fail the work.

Build order matters: get one manual run reliable, turn that procedure into a skill, wrap it in a loop, then schedule it. The practical success metric is not tasks attempted or tokens spent; it is **cost per accepted change**.

## Use Cases

Good candidates for agentic coding loops:

- babysitting a pull request until CI passes and review comments are resolved;
- repeatedly checking a deployment or build until it completes;
- migrating an API until every call site compiles and tests pass;
- splitting files or modules until size and test constraints are satisfied;
- processing an issue backlog until the queue is empty;
- opening or maintaining pull requests across multiple repositories;
- running continuous background code review and feeding findings back while context is fresh;
- supervising a set of specialized coding agents through a durable workspace manager.

Poor candidates:

- tasks with unclear stopping conditions;
- tasks where failure is expensive and no sandbox or rollback exists;
- irreversible actions without explicit human approval;
- work where correctness cannot be checked by tests, source citations, or review gates;
- broad "make it better" prompts with no budget or cap.

## Why It Matters

The loop reframes the engineer's role from one-off prompt writer to workflow designer. The source article's strongest practical claim is that reusable loops should be paired with reusable skills: the loop provides cadence and control; skills provide the practiced procedures the loop calls.

The cost center also changes. If a coding agent can produce code cheaply, the expensive part becomes managing the repeated agent work: tokens, runtime, attention, CI capacity, and risk. A production loop therefore needs explicit operational controls:

- maximum iterations;
- no-progress detection;
- token or dollar budget ceilings;
- deterministic verification before another iteration;
- clear rollback or branch isolation;
- human escalation when confidence is low.

Without those controls, loops become machines for producing confident mistakes, runaway bills, or noisy pull requests.

## Related Tools or Alternatives

- [Claude Code `/goal`](https://code.claude.com/docs/en/goal.md): session-scoped completion-condition loop that continues across turns until a validator model decides the condition is met.
- [Claude Code `/loop`](https://code.claude.com/docs/en/scheduled-tasks.md): scheduled prompt repetition for polling, PR maintenance, deployment checks, and in-session background maintenance.
- [Claude Dynamic Workflows](claude-dynamic-workflows.md): JavaScript-orchestrated subagent runs for multi-stage work with script-held intermediate state.
- [OpenAI Codex Automations](https://developers.openai.com/codex/app/automations.md): recurring background tasks with triage inbox behavior and optional skill calls.
- [OpenAI Codex Worktrees](https://developers.openai.com/codex/app/worktrees.md): Git-worktree-backed isolation for parallel/background Codex work.
- [OpenAI Codex Skills](https://developers.openai.com/codex/skills.md): reusable workflow format based on `SKILL.md`, optional scripts, references, and progressive disclosure.
- [OpenAI Codex Subagents](https://developers.openai.com/codex/subagents.md): explicitly requested parallel specialized agents with consolidated results.
- [ReAct](https://react-lm.github.io/): early reasoning-and-acting pattern where a model alternates reasoning, actions, and observations.
- [AutoGPT](https://github.com/Significant-Gravitas/AutoGPT): early autonomous-agent project that popularized goal-directed repeated agent loops.
- [Ralph Wiggum loop](https://ghuntley.com/ralph/): Geoffrey Huntley's simple repeated-agent-loop pattern; useful as a minimal reference point.
- [Gas Town](https://github.com/gastownhall/gastown): multi-agent workspace manager with persistent work tracking, git-backed hooks, mailboxes, identities, and handoffs. GitHub metadata checked on 2026-06-09 showed MIT license and 15k+ stars.
- Cron jobs and scheduled tasks: useful for fixed polling; become agentic only when the body includes model-based judgment, verification, and adaptive next-step decisions.

## Implementation Pattern

A safe implementation should specify scope, inputs, allowed tools, verification commands, durable state, budget caps, exit rules, and rollback. The minimal prompt shape is: goal, target scope, allowed actions, verification checks, stop condition, early-stop risks, and final report requirements.

## Risks and Guardrails

- **Infinite or low-progress loops**: use iteration caps and no-progress detection.
- **Billing surprises**: set token or dollar budgets before long-running runs.
- **Bad commits compounding**: run verification after each meaningful change and review diffs before merge.
- **Permission drift**: keep destructive commands out of auto-approved tool lists.
- **Context rot**: refresh state from git, CI, issue trackers, and tests rather than relying on old conversation context.
- **Over-broad autonomy**: isolate runs in branches or worktrees and require explicit approval before publishing, deleting, deploying, or merging.
- **Comprehension debt**: read diffs and spot-check gates; otherwise the repo can accumulate code the team no longer understands.
- **Cognitive surrender**: loop design should preserve engineering judgment, not outsource judgment calls to an unattended pipeline.
- **Skills as injection vectors**: do not auto-install community skills into unattended loops; audit the skill source and permission needs first.
- **Credential leakage**: sanitize logs, avoid verbose secret-bearing output, and scope credentials tightly for loop-specific jobs.
- **Permission scope creep**: periodically re-audit write permissions, deploy permissions, and connector scopes.

## Notes

- The source article argues that "cron plus a decision-maker in the body" is the honest middle ground: scheduling alone is not new, but model-driven decisions inside a scheduled loop are the new operational concern.
- The most reusable asset is often the skill or command called by the loop, not the loop itself.
- This note treats social-post claims about individual usage, view counts, and PR counts as source context unless separately confirmed by official documentation or repository metadata.
- Addy's most important warning is that loop design can either preserve engineering judgment or accelerate cognitive surrender. The loop should increase leverage while the engineer still reviews, understands, and owns what ships.
- Codez's roadmap is useful because it names the negative case: if the task does not repeat, cannot be checked objectively, lacks a reproduction environment, or cannot tolerate token waste, keep it as a manual prompt.
- The diagram set in the Codez article usefully distinguishes a prompt loop from an agentic loop: human-prompted work leaves the human as the loop; `/goal`, automations, state, and gates let the system close more of the loop while still keeping human approval at irreversible boundaries.

## Sources

- [Matt Van Horn X status](https://x.com/mvanhorn/status/2063865685558903149) — source post linking to the X Article.
- [X Article: WTF Is a Loop? Peter Steinberger vs. Boris Cherny](https://x.com/i/article/2063850827694096385) — source article and synthesis of the loop discourse.
- [Addy Osmani X status](https://x.com/addyosmani/status/2064127981161959567) — source post linking to the X Article.
- [X Article: Loop Engineering](https://x.com/i/article/2064122477731852288) — source article framing loop engineering as designing the system that prompts, verifies, and remembers agent work.
- [Codez X status](https://x.com/0xCodez/status/2064374643729773029) — source post linking to the X Article.
- [X Article: Loop engineering: the 14-step roadmap from prompter to loop designer](https://x.com/i/article/2064357550225510400) — source article and practical adoption checklist for loop engineering.
- [Claude Code docs: Keep Claude working toward a goal](https://code.claude.com/docs/en/goal.md) — official `/goal` behavior, completion condition, and evaluator model framing.
- [Claude Code docs: Run prompts on a schedule](https://code.claude.com/docs/en/scheduled-tasks.md) — official `/loop` and scheduled-task behavior.
- [Claude Code docs: Dynamic workflows](https://code.claude.com/docs/en/workflows.md) — official multi-agent workflow runtime and limits.
- [OpenAI Codex Automations docs](https://developers.openai.com/codex/app/automations.md) — official recurring-background-task behavior and triage inbox model.
- [OpenAI Codex Worktrees docs](https://developers.openai.com/codex/app/worktrees.md) — official Git worktree isolation behavior.
- [OpenAI Codex Skills docs](https://developers.openai.com/codex/skills.md) — official skill format and progressive disclosure model.
- [OpenAI Codex Subagents docs](https://developers.openai.com/codex/subagents.md) — official subagent workflow behavior and token-cost caveat.
- [ReAct project page](https://react-lm.github.io/) — source for reasoning-and-acting lineage.
- [AutoGPT GitHub repository](https://github.com/Significant-Gravitas/AutoGPT) — early autonomous-agent loop reference.
- [Geoffrey Huntley: Ralph Wiggum as a software engineer](https://ghuntley.com/ralph/) — Ralph loop reference.
- [Gas Town GitHub repository](https://github.com/gastownhall/gastown) — multi-agent workspace manager reference.
