---
tags: [type/reference, domain/software-engineering, domain/ai, topic/agent, topic/cli, workflow/debugging, workflow/documentation, status/reference]
source_link: "https://code.claude.com/docs/en/best-practices"
context_link: "https://code.claude.com/docs/en/best-practices"
source_type: "official-documentation"
kind: "workflow"
created: 2026-06-09
updated: 2026-06-09
canonical_name: "Best Practices for Claude Code"
research_sources:
  - "https://code.claude.com/docs/en/best-practices"
  - "https://code.claude.com/docs/en/overview"
  - "https://code.claude.com/docs/en/memory"
  - "https://code.claude.com/docs/en/prompt-caching"
last_verified_at: 2026-06-09
unmapped_terms: []
---

# Best Practices for Claude Code

## Summary

Anthropic’s Claude Code best-practices guide treats agentic coding as an execution loop that must be grounded by verification, explicit context, and aggressive context management. Claude Code can read files, run commands, edit code, and work autonomously, but the guide’s core constraint is that the context window fills quickly and model performance degrades as it fills.

The operating model is: give the agent a check it can run, let it explore before planning, keep prompts specific, configure project memory and permissions, steer early, and use subagents or parallel sessions for investigation and scale.

## What It Is

An official Claude Code documentation page covering practical patterns for using Claude Code effectively across codebases, local environments, MCP integrations, hooks, skills, subagents, and autonomous sessions.

## What It Does

- Describes how to close the agent loop with runnable verification: tests, builds, screenshots, lint checks, or other pass/fail signals.
- Recommends “explore first, then plan, then code” so the agent understands the codebase before changing it.
- Shows how to provide project context through CLAUDE.md, rich prompts, and environment configuration.
- Covers permissions, CLI tooling, MCP servers, hooks, skills, subagents, plugins, checkpoints, resume flows, non-interactive mode, multiple sessions, and adversarial review.

## How It Works

The guide’s discipline is built around turning vague coding requests into observable workflows:

1. Verification first: give Claude something executable that can determine whether the work is correct.
2. Discovery before edits: ask for codebase exploration and a plan before implementation.
3. Context control: provide relevant files, screenshots, logs, and commands, but manage the context window because it is the limiting resource.
4. Environment setup: use CLAUDE.md, permissions, CLI tools, MCP servers, hooks, skills, custom subagents, and plugins to make correct behavior easier.
5. Session management: course-correct early, rewind with checkpoints, resume conversations, and split work across subagents or multiple sessions where appropriate.
6. Quality control: add an adversarial review step before accepting changes.

## Use Cases

- Running Claude Code as a coding partner for non-trivial feature work.
- Creating repeatable verification harnesses so the agent can self-correct.
- Configuring repository instructions and local tools for more reliable agent behavior.
- Scaling investigation across files, branches, or multiple parallel sessions.
- Reducing context-window failure modes in long debugging sessions.

## Why It Matters

The page makes the difference between “chat with a coding model” and “operate an agentic coding environment.” Its most reusable lesson is that autonomous coding only becomes reliable when the agent can verify work without waiting for the human to inspect every result.

## Related Tools or Alternatives

- [[Claude Code Skills Lessons]] for how reusable skills can encode team-specific gotchas, verification, and runbooks.
- [[Claude Prompting Best Practices]] for model-level prompting principles behind agent behavior.
- [[OpenAI Prompt Caching]] for the cost/latency implications of stable prompt prefixes and long reusable context.

## Source Context

- Source: official Claude Code docs.
- Extraction status: page fetched successfully; full heading list and page text were available.
- Key source emphasis: context window management, executable verification, project instructions, subagents, checkpoints, and automation.
- Research enrichment: related official Claude Code overview, memory, and prompt-caching docs were checked as canonical context.

## Notes

Practical checklist:

- Define the pass/fail check before implementation.
- Tell the agent to inspect relevant files before making claims or edits.
- Keep static project guidance in CLAUDE.md or skills rather than repeating it manually.
- Use permissions and checkpoints for rollback.
- Use subagents for broad investigation and adversarial review, but keep final integration verified by tests or a build.

## Sources

- [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices)
- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code memory](https://code.claude.com/docs/en/memory)
- [Claude Code prompt caching](https://code.claude.com/docs/en/prompt-caching)
