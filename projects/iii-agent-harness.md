---
tags:
  - type/project
  - domain/ai
  - domain/automation
  - domain/software-engineering
  - topic/agent
  - topic/api
  - topic/integration
  - workflow/automation
  - workflow/documentation
  - status/reference
source_link: https://x.com/mfpiccolo/status/2060069083878408689
context_link: https://x.com/i/article/2060024515619397638
source_type: x-article
kind: project
created: "2026-06-09"
updated: "2026-06-09"
canonical_name: iii Agent Harness
research_sources:
  - https://x.com/mfpiccolo/status/2060069083878408689
  - https://x.com/i/article/2060024515619397638
  - https://github.com/iii-hq/iii
  - https://github.com/iii-hq/workers
  - https://raw.githubusercontent.com/iii-hq/iii/main/README.md
  - https://raw.githubusercontent.com/iii-hq/workers/main/README.md
  - https://raw.githubusercontent.com/iii-hq/workers/main/harness/README.md
  - https://iii.dev/docs
  - https://workers.iii.dev
last_verified_at: "2026-06-09"
unmapped_terms:
  - iii.trigger
  - worker bus
  - turn-orchestrator
  - hook-fanout
---

# iii Agent Harness

## Summary

The iii Agent Harness is Mike Piccolo's proposed architecture for building production agent systems as a composition of independently replaceable workers instead of adopting a monolithic agent framework. The triggering X Article argues that a harness is not one thing: provider routing, credentials, model catalog, session storage, policy checks, approvals, budgets, hooks, context compaction, streaming, and observability are separate jobs that should be swappable one layer at a time.

The canonical implementation lives around the `iii-hq/iii` engine and `iii-hq/workers` repository. Official README material describes iii as a runtime for composing, extending, and observing services through three primitives: **workers**, **functions**, and **triggers**. The harness package in `iii-hq/workers` bundles turn orchestration, approvals, sessions, providers, context compaction, and budgets as iii workers.

## Source Context

- Source post: Mike Piccolo / `@mfpiccolo`, linking to the X Article **"How to build your own agent harness???"**.
- Article ID: `2060024515619397638`; status ID: `2060069083878408689`.
- Author context: Mike Piccolo, founder of iii.
- Source thesis: agent teams often adopt harnesses as single frameworks; iii tries to decompose the harness into worker-level concerns connected by one shared trigger/function substrate.
- Social metrics and article claims are source-reported unless independently validated by official docs or GitHub metadata.

## What It Is

iii is an open-source backend/runtime substrate where services register as workers over a shared engine. The README describes the mental model as **Worker + Function + Trigger**:

- **Workers** are processes that connect to the iii engine and register functions/triggers.
- **Functions** are stable units of work, such as `provider::<name>::stream`, `policy::check_permissions`, or `approval::resolve`.
- **Triggers** are the events that cause functions to run: direct calls, HTTP, queues, cron, state changes, stream events, or other routing events.

The agent harness is a stack of these workers. Instead of forking a framework when one layer does not fit, the goal is to replace a worker that registers the same function IDs and keep the rest of the stack stable.

## What It Does

The harness exists to run agent turns durably, safely, and observably. The source article lists the production jobs it needs to cover:

- accept and persist turn requests;
- resolve model-provider credentials;
- know model capabilities such as tools, vision, streaming, and context windows;
- drive a turn state machine;
- load skill bodies and assemble system prompts;
- stream model output back to clients;
- check tool/function calls against policy;
- pause calls that need human approval and resume the right turn;
- track LLM spend against workspace or agent budgets;
- run before/after hooks;
- persist sessions as branching trees;
- compact history as context fills;
- emit event streams to the UI;
- carry OpenTelemetry traces across every step.

The official `workers` README confirms a `harness` module that bundles provider registry, configuration-backed credentials/settings/permissions, `turn-orchestrator`, `approval-gate`, `session`, `hook-fanout`, `models-catalog`, provider workers, `llm-budget`, and `context-compaction` as a TypeScript monorepo.

## How It Works

A typical turn in the source article is described as follows:

1. A client submits a turn through `harness::trigger` with session, message, and payload metadata.
2. The harness forwards to `run::start`, seeding tracing baggage so session/message IDs propagate through nested `iii.trigger` calls.
3. `turn-orchestrator` persists the request and drives a durable state machine.
4. Provisioning may start a sandbox, download skills, assemble the system prompt, and load live function schemas from the engine catalog.
5. Provider workers stream model output using credentials from an auth worker.
6. Tool calls route through a central policy/approval chokepoint before dispatch.
7. Approval decisions are persisted to iii state and wake the relevant turn through a shared state trigger.
8. Hook fanout, context compaction, budget recording, and event streaming run as neighboring workers on the same bus.
9. The system either loops back for another assistant turn, stops cleanly, or fails with an error event.

The official harness README frames this as: agents are workers, tools are functions, and handoffs use the same triggers and queues as the rest of the system.

## Use Cases

- Building an agent harness that must evolve beyond a single framework's defaults.
- Swapping a model provider, credential backend, policy engine, approval UI, or model catalog without rewriting the orchestrator.
- Running thin internal research agents with only a few workers installed.
- Running thick production agents with policy, approvals, budgets, traceability, context compaction, and audit controls.
- Integrating agent runtime concerns with existing backend primitives such as queues, HTTP, cron, state, storage, shell, database, and streaming.
- Giving agents a live catalog of functions/workers they can discover and call.

## Why It Matters

The architecture reframes the agent harness as a **slider** rather than a framework choice. A thin harness can include only an orchestrator, provider, auth, and minimal metadata. A thick harness can add approvals, budgets, custom policies, Slack approval surfaces, context compaction, hook subscribers, and finance-grade spend tracking. The same bus protocol and tracing model stay in place.

This matters because real agent systems often discover late that one layer is wrong: the policy engine is too rigid, approvals need to happen outside the chat UI, credentials need a real secrets manager, or budget tracking must map to finance dashboards. If those are framework internals, teams fork or rewrite. If they are workers, the replacement boundary is smaller.

## Related Tools or Alternatives

- **LangChain / LangGraph**: agent framework and graph orchestration approaches; often more framework-centered than worker-substrate-centered.
- **OpenAI Agents SDK / Anthropic Agent SDK**: SDK-level primitives for agent applications and provider-native agent integrations.
- **CrewAI / AutoGen**: multi-agent frameworks with their own coordination models.
- **Claude Dynamic Workflows**: script-orchestrated subagent workflows inside Claude Code; related conceptually, but not the same backend substrate.
- **MCP**: tool exposure protocol; iii has a worker that can expose iii functions tagged for MCP as tools.
- **Traditional service buses/queues**: can cover parts of routing and scheduling, but iii's claim is unified function discovery, triggers, tracing, and worker composition across services and agents.

## Official Project Metadata

- `iii-hq/iii`: public GitHub repo for the iii engine; GitHub API checked 2026-06-09 showed Rust as primary language, homepage `https://iii.dev`, and 17k+ stars.
- `iii-hq/workers`: public GitHub repo for worker modules; GitHub API checked 2026-06-09 showed TypeScript as primary language, Apache-2.0 license, and homepage `https://workers.iii.dev/`.
- `workers/harness`: TypeScript harness stack inside `iii-hq/workers`; official README lists fifteen workers across orchestration, governance, sessions, context, models, and cost.
- Install path from official docs: install iii, add workers with `iii worker add <name>`, and run the engine with a config.

## Risks and Guardrails

- **Moving target**: repository and worker names are actively evolving; verify current docs before implementation.
- **Operational complexity**: decomposition gives flexibility but increases the number of deployed pieces to monitor.
- **Contract discipline**: swappability depends on stable function IDs and payload schemas; undocumented drift would weaken the model.
- **Security**: provider credentials, policy fail-closed behavior, approval queues, and shell/database workers need careful configuration before production use.
- **Observability dependence**: the architecture relies heavily on consistent trace propagation; broken spans make debugging harder.
- **Source bias**: the triggering article is written by the project's founder, so architectural claims should be balanced with hands-on validation before adoption.

## Notes

The practical mental model: do not ask, "Which harness framework do we adopt?" Ask, "Which harness jobs do we need now, and which workers own them?" If a layer becomes wrong, replace that worker rather than forking the whole harness.

## Sources

- [Mike Piccolo X status](https://x.com/mfpiccolo/status/2060069083878408689) — source post linking to the X Article.
- [X Article: How to build your own agent harness???](https://x.com/i/article/2060024515619397638) — source article and architecture argument.
- [iii engine GitHub repository](https://github.com/iii-hq/iii) — canonical engine repo and metadata.
- [iii engine README](https://raw.githubusercontent.com/iii-hq/iii/main/README.md) — official Worker/Function/Trigger model and quickstart.
- [iii workers GitHub repository](https://github.com/iii-hq/workers) — canonical workers repo and metadata.
- [iii workers README](https://raw.githubusercontent.com/iii-hq/workers/main/README.md) — worker registry, module list, SDKs, and release model.
- [iii harness README](https://raw.githubusercontent.com/iii-hq/workers/main/harness/README.md) — official harness package description and worker concerns.
- [iii docs](https://iii.dev/docs) — official docs entry point.
- [iii worker registry](https://workers.iii.dev) — official worker registry.
