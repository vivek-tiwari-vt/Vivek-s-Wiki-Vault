---
tags:
  - type/workflow
  - domain/ai
  - domain/software-engineering
  - topic/agent
  - topic/llm
  - workflow/automation
  - workflow/research
  - status/reference
source_link: https://x.com/PawelHuryn/status/2064079508689358857
context_link: https://x.com/i/article/2064068045883228160
source_type: x-article
kind: workflow
created: "2026-06-09"
updated: "2026-06-09"
canonical_name: Claude Dynamic Workflows
research_sources:
  - https://x.com/PawelHuryn/status/2064079508689358857
  - https://x.com/i/article/2064068045883228160
  - https://x.com/trq212/status/2061907337154367865
  - https://x.com/i/article/2061850535708483585
  - https://code.claude.com/docs/en/workflows.md
  - https://claude.com/blog/introducing-dynamic-workflows-in-claude-code
  - https://claude.com/blog/a-harness-for-every-task-dynamic-workflows-in-claude-code
  - https://www.productcompass.pm/p/claude-code-dynamic-workflows
last_verified_at: "2026-06-09"
unmapped_terms:
  - ultracode
---

# Claude Dynamic Workflows

## Summary

Claude Dynamic Workflows are Claude Code runs where Claude writes a JavaScript orchestration script, then a workflow runtime executes that script to coordinate many subagents. The key shift is that branching, loops, filtering, scoring, and intermediate state move out of the model's conversation context and into deterministic code, while subagents still perform judgment-heavy work.

Paweł Huryn's X article frames the feature for product managers: he describes a product-discovery run where 113 agents processed 100 synthetic customer interviews, used 1.95M tokens, clustered 622 raw opportunities into 11 needs, and produced 3 verified prototypes in roughly 12 minutes. Treat those run metrics as source-reported, not independently reproduced here.

## Source Context

- Triggering post: Paweł Huryn shared an X post that links to an X Article titled **"Claude Dynamic Workflows (not only) for PMs: The Ultimate Guide"**.
- Author: Paweł Huryn / `@PawelHuryn`.
- Article ID: `2064068045883228160`; tweet/status ID: `2064079508689358857`.
- Additional canonical source: Thariq Shihipar / `@trq212` shared **"A harness for every task: dynamic workflows in Claude Code"**, also available on the Claude blog.
- Thariq article ID: `2061850535708483585`; tweet/status ID: `2061907337154367865`.
- Source claim: the practical value is that **"the model did the judgment, the code did the coordination."**
- External full guide: Product Compass article, **"Dynamic Workflows for PMs: Orchestrate AI Agents in Claude Code"**.

## What It Is

A dynamic workflow is a Claude Code workflow that uses a generated JavaScript script as the orchestrator. Official Claude Code docs describe it as a script Claude writes for a task and a runtime executes in the background while the session stays responsive.

The workflow sits between simpler subagent delegation and a manually built agent application:

- **Subagents**: useful for a few parallel tasks where Claude remains the turn-by-turn coordinator.
- **Skills**: reusable instructions Claude follows inside the conversation context.
- **Agent teams**: lead-agent style supervision of peer sessions.
- **Workflows**: orchestration lives in a script; intermediate results live in script variables; the runtime can coordinate dozens to hundreds of agents.

## What It Does

Dynamic workflows are meant to coordinate multi-stage agent work where later stages depend on earlier outputs. The workflow can:

- fan out work across many subagents;
- collect and normalize intermediate results;
- deduplicate and filter candidates;
- route items to different follow-up stages;
- score outputs with deterministic formulas when possible;
- spawn independent verifier or judge agents;
- restart or rerun failed/low-confidence stages;
- return one final synthesis instead of flooding the user's context window with every intermediate turn.

Official docs list `/deep-research` as a bundled workflow that fans out searches, fetches and cross-checks sources, votes on claims, and returns a cited report with unsupported claims filtered out.

## How It Works

The operating pattern is:

1. The user requests a workflow directly, asks Claude to "use a workflow," or uses the `ultracode` trigger/effort setting.
2. Claude writes a JavaScript workflow script for the described task.
3. Claude Code asks for approval where the current permission mode requires it.
4. The workflow runtime executes the script in the background.
5. The script spawns subagents, tracks their results, loops, branches, and coordinates stages.
6. Subagents use the model for reasoning, tool use, source reading, coding, verification, or judgment.
7. The script performs deterministic coordination such as loops, filters, sorts, counters, routing, and stop conditions.
8. Progress is managed through `/workflows`, where the user can inspect phases, agents, token counts, elapsed time, pause/resume, stop, restart selected agents, or save the workflow.

Important limits and constraints from the official docs:

- Dynamic workflows require Claude Code v2.1.154 or later.
- They are available on paid plans and supported provider routes listed in the docs.
- The runtime has no direct filesystem or shell access; agents perform reads, writes, and commands while the script coordinates them.
- Workflows can run up to 16 concurrent agents and up to 1,000 agents total per run.
- Runs can use meaningfully more tokens than a normal conversation because they spawn many agents.
- Saved workflows can live in `.claude/workflows/` for a project or `~/.claude/workflows/` for personal reuse.

## Use Cases

Good workflow candidates are tasks where one stage's output decides the next stage, or where independent verification improves reliability:

- codebase-wide bug hunts, auth audits, security checks, and profiler-guided optimization audits;
- large migrations, framework swaps, API deprecations, and modernization passes across many files;
- deep research where claims should be cross-checked across several sources;
- product-discovery synthesis across many interviews, surveys, support tickets, or sales notes;
- PRD, blog post, or requirements red-team checks against source material and code;
- root-cause investigations where separate agents inspect logs, files, data, and hypotheses;
- triage at scale for incidents, support queues, bug reports, or recurring root causes;
- qualitative sorting and pairwise ranking, such as resumes, tickets, or candidate ideas;
- mining session history or review comments for recurring corrections and converting them into rules;
- model/intelligence routing where a classifier chooses Sonnet, Opus, or smaller models by task complexity;
- idea generation followed by filtering, judging, and prototype generation;
- repetitive review workflows that should be saved and rerun as commands.

Paweł's PM-oriented mental model is useful: use a subagent when the job is one round of parallel judgment; use a workflow when the steps need to talk to each other.

## Why It Matters

The feature changes the cost and reliability profile of long agent tasks. Instead of keeping the whole plan in the model context, the workflow script can hold the plan, intermediate variables, loops, and branching. That helps with three failure modes highlighted in the source article:

- **Laziness / incomplete execution**: code loops can continue until the input list is empty.
- **Self-preferential bias**: separate judge agents can check work instead of letting the producing agent grade itself.
- **Goal drift**: constraints embedded in the script are less likely to disappear as the conversation context grows.

For recurring knowledge work, the payoff is not merely "more agents." It is repeatable orchestration: a workflow can become a saved command that encodes the process, acceptance gates, and stopping conditions.

## Patterns To Recognize

The X article names six reusable shapes:

1. **Classify-and-act**: one agent classifies an item; the script routes it.
2. **Fan-out-and-synthesize**: one agent per item or source; code merges the results.
3. **Adversarial verification**: independent agents check an output against a rubric.
4. **Generate-and-filter**: many candidates are produced, deduplicated, ranked, and narrowed.
5. **Tournament / compare**: multiple agents solve the same task differently; judges compare results.
6. **Loop-until-done**: the workflow keeps spawning or rerunning work until a stop condition is met.

## Related Tools or Alternatives

- **Claude Code subagents**: better for smaller, bounded delegation where Claude can coordinate in the normal turn loop.
- **Claude Code skills**: better for reusable instructions and operating procedures, not necessarily large-scale orchestration.
- **Claude Agent SDK**: better when building agent behavior into an application or product; dynamic workflows are aimed at workspace work inside Claude Code.
- **n8n / workflow automation tools**: better for known tool-to-tool automations; dynamic workflows are more useful when the procedure itself should be synthesized for the current task.
- **Manual scripts**: more predictable for stable, fully specified jobs; workflows are useful when many parts still require model judgment.

## Risks and Guardrails

- **Cost**: many agents can consume a large number of tokens. Start with a small slice before running over an entire repo or large document set.
- **Permissions**: workflow-spawned subagents inherit tool allowlists and can perform file edits in `acceptEdits` mode. Review the plan and tool permissions before launching large unattended runs.
- **Runaway scope**: use explicit stop conditions, spend caps, target paths, and success criteria.
- **False confidence**: parallelism does not guarantee correctness; include independent verification and source citation stages.
- **Operational containment**: for real work, run in a clean branch/worktree, sandbox, or disposable environment when file edits and shell commands are allowed.

## Notes

- `ultracode` can be used as a prompt trigger or effort setting in Claude Code. In official docs, the setting combines `xhigh` reasoning effort with automatic workflow orchestration for substantive tasks.
- Workflows are inspectable and reusable: the script is written under the session directory, can be opened, and can be saved as a command.
- Thariq's article emphasizes prompt specificity: ask for the pattern you want, such as quick adversarial review, tournament, fan-out-and-synthesize, or loop-until-done.
- Repeated workflows can be paired with `/loop`; hard completion requirements can be expressed with `/goal`.
- Token budgets can be prompted explicitly, for example asking the workflow to use a fixed cap such as `10k tokens`.
- Saved workflow JavaScript can be distributed directly via `.claude/workflows/` or packaged inside a skill as a reusable template.
- The Product Compass guide appears to expand the PM playbook beyond the X Article, including how to build, run, contain, and decide when not to use a workflow.

## Sources

- [Paweł Huryn X status](https://x.com/PawelHuryn/status/2064079508689358857) — source post linking to the X Article.
- [X Article: Claude Dynamic Workflows (not only) for PMs: The Ultimate Guide](https://x.com/i/article/2064068045883228160) — source article and PM framing.
- [Thariq X status](https://x.com/trq212/status/2061907337154367865) — source post linking to the Anthropic-authored X Article.
- [X Article: A harness for every task: dynamic workflows in Claude Code](https://x.com/i/article/2061850535708483585) — Anthropic/Claude Code team framing on patterns, use cases, and tips.
- [Claude Code docs: Orchestrate subagents at scale with dynamic workflows](https://code.claude.com/docs/en/workflows.md) — official feature definition, usage, limits, controls, and cost notes.
- [Claude blog: A harness for every task](https://claude.com/blog/a-harness-for-every-task-dynamic-workflows-in-claude-code) — official blog version of Thariq and Sid Bidasaria's article.
- [Claude blog: Introducing dynamic workflows](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code) — official launch context.
- [The Product Compass: Dynamic Workflows for PMs](https://www.productcompass.pm/p/claude-code-dynamic-workflows) — expanded guide referenced by the X Article.
