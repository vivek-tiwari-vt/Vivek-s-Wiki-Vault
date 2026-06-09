---
tags: [type/reference, domain/ai, domain/software-engineering, topic/agent, topic/cli, workflow/documentation, workflow/automation, status/reference]
source_link: "https://claude.com/blog/lessons-from-building-claude-code-how-we-use-skills"
context_link: "https://claude.com/blog/lessons-from-building-claude-code-how-we-use-skills"
source_type: "official-blog"
kind: "workflow"
created: 2026-06-09
updated: 2026-06-09
canonical_name: "Lessons from Building Claude Code: How We Use Skills"
research_sources:
  - "https://claude.com/blog/lessons-from-building-claude-code-how-we-use-skills"
  - "https://code.claude.com/docs/en/skills"
  - "https://platform.claude.com/docs/en/agents-and-tools/skills/overview"
last_verified_at: 2026-06-09
unmapped_terms: []
---

# Claude Code Skills Lessons

## Summary

Anthropic’s blog post explains that skills are not merely markdown prompts; they are folders of instructions, scripts, references, assets, and configuration that agents can discover and use. The strongest skills encode non-obvious local knowledge: gotchas, verification harnesses, data-fetching patterns, scaffolds, runbooks, deployment procedures, and operational guardrails.

The main design lesson is progressive disclosure: keep the top-level skill concise, link to references/scripts/templates when needed, and let the agent load more detail only for the current situation.

## What It Is

An official Claude blog post about how Anthropic uses skills internally with Claude Code, including skill categories, authoring practices, distribution, composition, and measurement.

## What It Does

- Defines skills as folders containing instructions, scripts, resources, and configuration.
- Categorizes internal Anthropic skills into nine patterns.
- Recommends high-signal authoring practices such as gotchas sections, progressive disclosure, setup guidance, and model-oriented descriptions.
- Discusses distributing, composing, and measuring skills.

## How It Works

The article identifies nine skill categories:

1. Library and API reference.
2. Product verification.
3. Data fetching and analysis.
4. Business process and team automation.
5. Code scaffolding and templates.
6. Code quality and review.
7. CI/CD and deployment.
8. Runbooks.
9. Infrastructure operations.

Authoring guidance preserved from the source:

- Do not state the obvious; focus on information the model would not infer from generic coding knowledge.
- Build a gotchas section from recurring model failures and operational footguns.
- Use files, folders, references, scripts, and templates for progressive disclosure.
- Avoid railroading the agent with overly narrow instructions.
- Think through setup, credentials, and prerequisites.
- Write descriptions for the model’s trigger decision, not for human marketing copy.
- Store scripts and generated code where deterministic execution beats prose.
- Use hooks when a skill should run at a particular point in the tool loop.
- Measure skill usage to find popular skills and under-triggering skills.

## Use Cases

- Building durable process memory for recurring engineering workflows.
- Capturing verification harnesses and gotchas for AI coding agents.
- Packaging organizational data-access, deployment, and runbook knowledge.
- Creating reusable templates and scripts that reduce repeated prompt burden.
- Auditing a skill library for missing categories or overbroad skills.

## Why It Matters

Skills are a practical way to convert repeated agent mistakes into reusable guardrails. They are also a better home than long prompts for team-specific procedures, because they can include executable scripts, examples, templates, and references that the model loads only when relevant.

## Related Tools or Alternatives

- [[Best Practices for Claude Code]] for agentic coding operations where skills, subagents, hooks, and verification interact.
- [[Claude Prompting Best Practices]] for the prompt-level principles that skills package into reusable form.
- [[OpenAI Prompt Caching]] for why stable reusable instruction prefixes and modular context can matter for cost and latency.

## Source Context

- Source: official Claude blog.
- Source date visible in extracted article: 2026-06-03.
- Extraction status: page fetched successfully; headings and full text were available.
- Research enrichment: official Claude Code skills docs and Claude platform agent-skills overview were checked.

## Notes

Skill-library audit questions:

- Which recurring workflows still depend on the user re-explaining context?
- Which classes of failure have no gotchas section?
- Which skills should contain deterministic scripts rather than prose instructions?
- Which skills are too broad and should be split into a clean category?
- Which skills under-trigger because their description is written for humans rather than the model?

## Sources

- [Lessons from building Claude Code: How we use skills](https://claude.com/blog/lessons-from-building-claude-code-how-we-use-skills)
- [Claude Code skills documentation](https://code.claude.com/docs/en/skills)
- [Claude agent skills overview](https://platform.claude.com/docs/en/agents-and-tools/skills/overview)
