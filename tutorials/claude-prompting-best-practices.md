---
tags: [type/reference, domain/ai, domain/software-engineering, topic/llm, topic/prompting, topic/agent, workflow/documentation, status/reference]
source_link: "https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices"
context_link: "https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices"
source_type: "official-documentation"
kind: "tutorial"
created: 2026-06-09
updated: 2026-06-09
canonical_name: "Claude Prompting Best Practices"
research_sources:
  - "https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices"
  - "https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview"
  - "https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview"
  - "https://platform.claude.com/docs/en/build-with-claude/extended-thinking"
last_verified_at: 2026-06-09
unmapped_terms: []
---

# Claude Prompting Best Practices

## Summary

Anthropic’s prompting best-practices guide is a broad operating manual for prompting Claude’s current models. It emphasizes clear and direct instructions, enough context to explain intent, examples for consistent output, XML-style structure for complex prompts, role setting, long-context organization, output-format control, tool-use guidance, thinking-depth control, and agentic safety.

For agentic systems, the strongest reusable guidance is to separate static context from variable input, explicitly bound autonomy, require investigation before claims, and steer tool use and subagent spawning rather than assuming the model will infer the desired operating style.

## What It Is

An official Claude API documentation page on prompt engineering techniques for Claude models, including standard prompting, long-context prompting, output control, tool use, thinking/reasoning, and agentic coding systems.

## What It Does

- Converts vague instructions into explicit, testable prompt structure.
- Shows how examples, XML tags, and role prompts improve consistency.
- Provides guidance for long-context work, quote extraction, formatting, continuations, and refusals.
- Covers tool-use prompting, parallel tool calls, autonomy/safety boundaries, subagent orchestration, and hallucination reduction in codebase work.

## How It Works

Core patterns from the source:

- Be clear and direct: specify the desired output format, constraints, order of operations, and “above and beyond” expectations explicitly.
- Add context: tell Claude why the behavior matters so it can generalize correctly.
- Use examples: include relevant, diverse, structured examples; 3–5 examples are recommended for many format/tone tasks.
- Use XML tags: wrap instructions, context, examples, documents, and inputs in consistent tags so Claude can parse complex prompts unambiguously.
- Put long-form data near the top for long-context tasks and put the query/instructions after the documents.
- Control tools: explicitly tell Claude when to use tools, how aggressively to parallelize independent calls, and when not to guess dependent parameters.
- Bound autonomy: encourage local reversible actions, but require confirmation for destructive, public, or hard-to-reverse actions.
- Reduce hallucination: instruct the model to inspect referenced files and evidence before answering.

## Use Cases

- Designing prompts for Claude API applications.
- Building reliable agent prompts for coding, research, and tool-using systems.
- Reducing hallucinations in codebase Q&A.
- Improving output-format reliability in reports, JSON-like schemas, and structured documents.
- Controlling model verbosity, effort, tool behavior, and safety boundaries.

## Why It Matters

Prompting failures often look like model limitations but are actually ambiguity, missing context, poor evidence discipline, or uncontrolled autonomy. This guide is valuable because it gives concrete levers for making Claude more literal, grounded, and operationally safe.

## Related Tools or Alternatives

- [[Best Practices for Claude Code]] for applying these principles inside an agentic coding environment.
- [[Claude Code Skills Lessons]] for packaging prompt and workflow knowledge as reusable skills.
- [[OpenAI Prompt Caching]] for the infrastructure implications of stable prompt prefixes and long shared instructions.

## Source Context

- Source: official Claude API docs.
- Extraction status: page fetched successfully; extracted headings covered prompting, tool use, reasoning, agentic systems, and migration considerations.
- Research enrichment: official Claude prompt engineering overview, tool-use overview, and extended-thinking documentation were checked.

## Notes

Reusable prompt snippets to preserve:

- Investigation rule: “Never speculate about code you have not opened. Read relevant files before answering codebase questions.”
- Safety rule: “Take local reversible actions freely, but ask before destructive, public, hard-to-reverse, or shared-infrastructure actions.”
- Parallelism rule: “Parallelize independent tool calls; never parallelize calls whose parameters depend on previous results.”

## Sources

- [Claude prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
- [Claude prompt engineering overview](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview)
- [Claude tool use overview](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview)
- [Claude extended thinking](https://platform.claude.com/docs/en/build-with-claude/extended-thinking)
