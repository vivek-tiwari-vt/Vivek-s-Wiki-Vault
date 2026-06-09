---
tags: [type/reference, domain/ai, domain/infrastructure, topic/llm, topic/api, workflow/retrieval, workflow/documentation, status/reference]
source_link: "https://developers.openai.com/api/docs/guides/prompt-caching"
context_link: "https://developers.openai.com/api/docs/guides/prompt-caching"
source_type: "official-documentation"
kind: "tutorial"
created: 2026-06-09
updated: 2026-06-09
canonical_name: "OpenAI Prompt Caching"
research_sources:
  - "https://developers.openai.com/api/docs/guides/prompt-caching"
  - "https://developers.openai.com/api/docs/guides/latency-optimization"
  - "https://developers.openai.com/api/docs/guides/cost-optimization"
  - "https://developers.openai.com/api/docs/pricing"
last_verified_at: 2026-06-09
unmapped_terms: []
---

# OpenAI Prompt Caching

## Summary

OpenAI Prompt Caching automatically reduces latency and input-token cost when requests share long exact prompt prefixes. The key design rule is to place stable content—system instructions, tool definitions, examples, schemas, images, and other repeated context—at the beginning of the prompt, while putting dynamic user-specific content at the end.

The page states that caching is automatic for eligible prompts, starts at prompts of 1024 tokens or more, and exposes `cached_tokens` in usage details so applications can measure cache hits.

## What It Is

An official OpenAI API guide explaining prompt caching behavior, prompt structure, cache routing, cache lookup, retention policies, cacheable content, and practical best practices for latency and cost reduction.

## What It Does

- Explains exact-prefix cache matching.
- Describes cache routing based on an initial prompt-prefix hash and optional `prompt_cache_key`.
- Documents cache lookup, cache hits, cache misses, and prompt-cache retention policies.
- Lists what can be cached: messages, images, tool definitions, and structured-output schemas.
- Shows how to inspect `usage.prompt_tokens_details.cached_tokens`.

## How It Works

OpenAI routes requests to infrastructure likely to have recently processed the same prompt prefix. If the selected machine has a matching cached prefix, the request can reuse cached computation for that prefix. If not, the full prompt is processed and the prefix may be cached afterward.

Important mechanics from the source:

- Exact prefix matches are required.
- Static prompt content should come first; variable request-specific content should come last.
- Caching applies automatically when prompts contain at least 1024 tokens.
- `prompt_cache_key` can improve routing for requests sharing common prefixes.
- If too many requests share a prefix/key combination—approximately above 15 requests per minute per the source—overflow routing may reduce cache effectiveness.
- In-memory retention generally lasts 5–10 minutes of inactivity and up to about one hour for supported models; extended retention can last longer, up to 24 hours for listed models.

## Use Cases

- Applications with long reusable system prompts.
- Agent systems with large stable tool schemas and instruction blocks.
- Multi-turn workflows where a static project, policy, or document context is reused.
- High-throughput API applications seeking lower latency and input-token cost.

## Why It Matters

Prompt caching turns prompt layout into infrastructure design. Stable prefixes become cheaper and faster; unstable prefixes break cache hits. For agent systems, this means reusable instructions, tool definitions, and schemas should be deliberately ordered and versioned rather than casually concatenated.

## Related Tools or Alternatives

- [[Claude Prompting Best Practices]] for prompt structure and long-context organization from the Claude side.
- [[Best Practices for Claude Code]] for agent workflows where long context and prompt caching are operational constraints.
- Retrieval systems such as [[Vector Databases for RAG]] when repeated context should be retrieved rather than placed permanently in every prompt.

## Source Context

- Source: official OpenAI API docs.
- Extraction status: page fetched successfully; source text contained structuring, routing, retention, requirements, cacheable content, and best-practice sections.
- Research enrichment: OpenAI latency optimization, cost optimization, and pricing pages were checked as related official context.

## Notes

Implementation checklist:

- Put stable system/developer instructions first.
- Keep tool definitions and structured-output schemas stable and early.
- Put user-specific or volatile request data last.
- Use `prompt_cache_key` consistently for shared-prefix workloads.
- Log `cached_tokens` and cache-hit rate; do not assume caching is working.
- Avoid unnecessary prompt-prefix changes such as timestamps, random IDs, or reordered tools before the cacheable prefix.

## Sources

- [OpenAI Prompt caching guide](https://developers.openai.com/api/docs/guides/prompt-caching)
- [OpenAI latency optimization guide](https://developers.openai.com/api/docs/guides/latency-optimization)
- [OpenAI cost optimization guide](https://developers.openai.com/api/docs/guides/cost-optimization)
- [OpenAI API pricing](https://developers.openai.com/api/docs/pricing)
