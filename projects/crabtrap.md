---
tags:
  - type/project
  - domain/security
  - domain/infrastructure
  - domain/ai
  - topic/agent
  - topic/api
  - topic/integration
  - workflow/automation
  - workflow/monitoring
  - status/reference
source_link: https://www.instagram.com/reel/DXdHzDWDLXr/?igsh=[REDACTED]
context_link: https://github.com/brexhq/crabtrap
official_link: https://github.com/brexhq/crabtrap
source_type: instagram_reel
kind: project
created: "2026-05-08 15:27:11"
updated: "2026-05-08 15:45:00"
canonical_name: CrabTrap
company: Brex
status: reference
research_sources:
  - https://github.com/brexhq/CrabTrap
  - https://www.brex.com/crabtrap
  - https://www.brex.com/journal/building-crabtrap-open-source
last_verified_at: "2026-05-08"
unmapped_terms: []
---

# CrabTrap

## Summary

CrabTrap is an outbound policy proxy for AI agents that sits between the agent runtime and the public internet. Its purpose is narrow but important: intercept every outbound HTTP or HTTPS request, evaluate it against deterministic rules and a natural-language policy enforced by an LLM judge, and then either allow or deny the call while recording the decision. That makes CrabTrap less like a general security gateway and more like a transport-layer control point for agent systems that need network access but cannot be trusted to call every endpoint safely.

## What It Is

CrabTrap is an open-source HTTP/HTTPS forward proxy from Brex for securing production AI agents. It is designed for agent-originated outbound traffic, not general web filtering or inbound application security.

## What It Does

- Intercepts outbound API requests from agents.
- Applies static rules first, then falls back to an LLM judge when no deterministic rule matches.
- Uses natural-language policies scoped to an agent identity.
- Logs requests, decisions, and outcomes into PostgreSQL for audit and replay.
- Ships a policy builder, eval system, and admin UI so teams can draft, test, and operate policies over time.

## How It Works

CrabTrap works by becoming the network chokepoint for the agent. Teams set `HTTP_PROXY` and `HTTPS_PROXY` so outbound requests flow through the proxy. CrabTrap can terminate TLS, inspect the request, apply deterministic allow or deny patterns, and escalate unmatched traffic to an LLM-based policy decision. The project's blog and README also describe a policy-builder workflow based on observed traffic and an eval system that replays historical entries against new draft policies. That combination makes it both an enforcement surface and a feedback system for refining what an agent is actually allowed to do.

## Use Cases

- Securing coding, research, or automation agents that must call external SaaS APIs with real credentials.
- Adding outbound traffic control in front of agents without rewriting each tool integration separately.
- Auditing agent network behavior and using the logs to tighten both policies and agent design.
- Running a staged security model where known-good traffic is approved cheaply through static rules and the long tail is judged contextually.

## Why It Matters

CrabTrap matters because agent safety problems often show up at the network boundary, where a prompt-injected or hallucinating agent can cause production effects using valid credentials. Many existing controls operate either at the tool layer or inside one vendor's stack. CrabTrap instead works at the transport layer, which makes it more framework-agnostic and operationally central. That is a meaningful design choice for teams trying to secure agents in production rather than only in demos.

## Related Tools or Alternatives

- MCP gateways and tool-layer permission systems when the goal is controlling structured tool calls rather than raw network requests.
- Egress-control or sandbox-layer products when the requirement is broader sandbox containment.
- Traditional WAFs and API gateways, which solve different traffic directions and threat models.

## Sources

- [CrabTrap GitHub repository](https://github.com/brexhq/CrabTrap)
- [CrabTrap product page](https://www.brex.com/crabtrap)
- [Brex engineering writeup on CrabTrap](https://www.brex.com/journal/building-crabtrap-open-source)

## Source Context

- Trigger source: Instagram reel from `githubsignals`
- Source framing: security guardrail for AI agents
- Canonical research source: Brex repo, product page, and engineering blog

## Notes

- The project explicitly states what it does not do, which is useful when comparing it with broader security products.
- This note is based on the official repo and Brex materials rather than the short-form reel caption.
