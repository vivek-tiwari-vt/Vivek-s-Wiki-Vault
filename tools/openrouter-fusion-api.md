---
tags:
  - type/tool
  - domain/ai
  - topic/llm
  - topic/api
  - workflow/research
  - status/reference
source_link: https://x.com/openrouter/status/2065856853989270011?s=52
context_link: https://openrouter.ai/blog/announcements/fusion-beats-frontier/
source_type: x-post
kind: tool
created: "2026-06-14"
updated: "2026-06-14"
canonical_name: OpenRouter Fusion API
research_sources:
  - https://x.com/OpenRouter/status/2065856853989270011
  - https://api.fxtwitter.com/openrouter/status/2065856853989270011
  - https://openrouter.ai/blog/announcements/fusion-beats-frontier/
  - https://openrouter.ai/docs/guides/features/server-tools/fusion
  - https://openrouter.ai/docs/guides/features/plugins/fusion
  - https://openrouter.ai/docs/guides/routing/routers/fusion-router
  - https://arxiv.org/abs/2602.11685
last_verified_at: "2026-06-14"
unmapped_terms:
  - Fable 5
  - GPT-5.5
  - Claude Opus 4.8
---

# OpenRouter Fusion API

## Summary

OpenRouter Fusion is a beta multi-model deliberation capability exposed through OpenRouter as a model alias (`openrouter/fusion`), a server tool (`openrouter:fusion`), and a plugin (`id: fusion`). The OpenRouter X post announced it as a “compound model” that can reach Fable-level deep-research performance at lower cost; OpenRouter’s own blog and docs frame it more precisely as a panel-and-judge pipeline for prompts that benefit from multiple model perspectives.

## Source Capture

- Source: OpenRouter X post, `https://x.com/OpenRouter/status/2065856853989270011`.
- Posted: 2026-06-13 18:00:30 UTC.
- Source text: “Introducing the Fusion API, the smartest compound model in the market. Fusion achieves Fable-level intelligence at half the price. How it works 👇”.
- Source metrics at capture time via `api.fxtwitter.com`: 581 replies, 1,512 retweets, 12,936 likes, 11,827 bookmarks, 1,051 quotes, and 4,671,707 views.
- Attached image: a benchmark bar chart comparing Fusion panels and solo models on DRACO-style deep-research tasks.

## What It Is

Fusion lets one request fan out to a panel of up to eight analysis models. The panel responses are then evaluated by a judge model, which returns structured analysis to the outer/calling model. The final answer is written by that outer model using the judge analysis.

OpenRouter documents three related entry points:

- `openrouter/fusion`: a model alias / Fusion Router that auto-injects the Fusion tool.
- `openrouter:fusion`: a beta server tool that can be added explicitly to a request’s `tools` array.
- `plugins: [{ "id": "fusion", ... }]`: a configuration surface for selecting panel models and judge model.

## How It Works

1. The user sends a prompt to an outer model or to the `openrouter/fusion` alias.
2. The outer model decides whether the prompt warrants deliberation; `tool_choice: "required"` can force Fusion.
3. A panel of models answers in parallel. OpenRouter docs say the panel runs with `openrouter:web_search` and `openrouter:web_fetch` enabled.
4. A judge model compares the panel responses and produces structured JSON analysis: consensus, contradictions, partial coverage, unique insights, and blind spots.
5. The outer model writes the final answer grounded in that structured analysis.

## API Usage

Simple model-alias form:

```json
{
  "model": "openrouter/fusion",
  "messages": [
    { "role": "user", "content": "What are the strongest arguments for and against carbon taxes?" }
  ]
}
```

Explicit server-tool form:

```json
{
  "model": "~anthropic/claude-opus-latest",
  "messages": [
    { "role": "user", "content": "Survey the strongest arguments for and against a carbon tax." }
  ],
  "tools": [
    { "type": "openrouter:fusion" }
  ]
}
```

Plugin customization form:

```json
{
  "model": "openrouter/fusion",
  "plugins": [
    {
      "id": "fusion",
      "analysis_models": [
        "~anthropic/claude-opus-latest",
        "~openai/gpt-latest",
        "~google/gemini-pro-latest"
      ],
      "model": "~openai/gpt-latest"
    }
  ]
}
```

## Benchmark Claims from OpenRouter

OpenRouter’s blog says it tested Fusion on 100 deep-research tasks from the DRACO benchmark and reported mean normalized scores. The X image and blog claim:

- Fable 5 + GPT-5.5 fused by Opus 4.8: 69.0%.
- Opus 4.8 + GPT-5.5 + Gemini 3.1 Pro fused by Opus 4.8: 68.3%.
- Opus 4.8 + GPT-5.5 fused by Opus 4.8: 67.6%.
- Opus 4.8 + Opus 4.8 self-fusion: 65.5%.
- Claude Fable 5 solo: 65.3%.
- Gemini 3 Flash + Kimi K2.6 + DeepSeek V4 Pro fused by Opus 4.8: 64.7%.
- Solo baselines shown: DeepSeek V4 Pro 60.3%, GPT-5.5 60.0%, Claude Opus 4.8 58.8%, Kimi K2.6 53.7%, Gemini 3.1 Pro 45.4%, Gemini 3 Flash 43.1%.

Important caveat: OpenRouter states that 7 of the 100 DRACO tasks were not completed for Fable 5 because content filters blocked execution, so Fable-related results reflect 93 scored tasks rather than the full 100. OpenRouter also says its DRACO implementation used Gemini 3.1 Pro Preview as judge instead of the DRACO paper’s original judge, so the scores are intended mainly as relative comparisons within this run.

## Best Uses

- Deep research questions where multiple perspectives reduce blind spots.
- Expert critique, adversarial review, and compare/contrast work.
- Architecture and strategy decisions where the cost of being wrong is higher than the cost of extra completions.
- Selective use from a coding agent for research-heavy architecture or best-practice questions, not routine code edits.

## Risks and Constraints

- Beta API: OpenRouter marks server tools as beta, so behavior and request shape may change.
- Cost and latency: OpenRouter’s FAQ says Fusion invocations are often 2–3x longer than a standard call and use multiple completions.
- Not a drop-in replacement for long-horizon coding or Fable-style agent tasks; OpenRouter specifically says it is not a drop-in replacement for Fable or coding models.
- Benchmark contamination risk: OpenRouter says it had to block DRACO rubric locations from `web_search` and `web_fetch` after discovering models could find grading rubrics online.
- Score claims are OpenRouter-reported, not independently reproduced in this ingest.

## Practical Takeaway

Fusion is best treated as a server-side deliberation primitive: call it when a single model’s answer needs independent perspectives, source checking, or structured disagreement analysis. For Master Vivek’s agent workflows, it is most relevant as an optional high-cost research/escalation tool rather than the default inference path.

## Related Links

- OpenRouter announcement post: https://x.com/OpenRouter/status/2065856853989270011
- OpenRouter Fusion chatroom: https://openrouter.ai/fusion
- OpenRouter blog announcement: https://openrouter.ai/blog/announcements/fusion-beats-frontier/
- Server tool docs: https://openrouter.ai/docs/guides/features/server-tools/fusion
- Plugin docs: https://openrouter.ai/docs/guides/features/plugins/fusion
- Fusion Router docs: https://openrouter.ai/docs/guides/routing/routers/fusion-router
- DRACO benchmark paper: https://arxiv.org/abs/2602.11685

## Uncertainties

- The benchmark values and model names are source-reported by OpenRouter and were not reproduced locally.
- The attached chart does not show exact axis labels beyond percentages; exact values above are from OpenRouter’s blog table, not solely OCR from the image.
- Future OpenRouter documentation may change the default panel, judge, and beta API details.
