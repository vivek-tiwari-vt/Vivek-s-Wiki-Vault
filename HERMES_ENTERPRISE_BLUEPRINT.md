---
title: Hermes Enterprise Autonomy Blueprint
owner: Vivek (Master Vivek)
created: "2026-06-14"
status: draft-for-review
scope: Turn the local Hermes setup into an enterprise-trustworthy autonomous agent platform
related:
  - workflows/agentic-coding-loops.md
  - workflows/hermes-standing-agent-prompts.md
  - workflows/claude-dynamic-workflows.md
  - projects/evo-autoresearch-workflow-loops.md
---

# Hermes Enterprise Autonomy Blueprint

> A build plan to evolve your existing `~/.hermes` install from a lightly-used
> personal agent into a fleet a company would trust at scale — with agent IAM,
> isolation, maker–checker verification, memory, observability, and budgeted
> autonomy. Designed around your stated preference: **monitor first, expand later.**

---

## 1. The frame: you are no longer prompting, you are designing loops

Your own wiki already nails the thesis (see `workflows/agentic-coding-loops.md`).
Peter Steinberger's line — *"you shouldn't be prompting coding agents anymore;
you should be designing loops that prompt your agents"* — and Addy Osmani's
"loop engineering" are the operating model here. A production loop is not
`while true`; it is a small operating system around the agent's work with six
moving parts:

1. **Trigger** — schedule or event that discovers work.
2. **Body** — what the agent reads, compares, drafts, or fixes.
3. **Verifier** — an *independent* grader (tests, CI, a checker subagent), not
   the same model grading itself (Lance Martin / Fable-5 rule).
4. **State** — durable memory of what was tried and what passed.
5. **Escalation** — when to notify, when to stay silent, when to ask approval.
6. **Guardrails** — iteration caps, budget ceilings, isolation, rollback,
   human approval at irreversible boundaries.

Your `workflows/hermes-standing-agent-prompts.md` note already reframes a
prompt-to-a-persistent-agent as a **job description = trigger + body +
escalation rule**. This blueprint is the engineering layer underneath that
framing.

### Where OpenClaw fits
"OpenClaw" (formerly **Clawdbot / Moltbot**) is the prior-generation personal
agent framework that Hermes positions itself as the successor to — Hermes ships
an official `hermes claw migrate` importer (`~/.openclaw/` → `~/.hermes/`) that
maps persona/`SOUL.md`, `MEMORY.md`/`USER.md`, skills, providers, sandbox
backend, and session-reset policy across. Practical takeaway: the *concepts*
(persona file, workspace memory, multi-provider routing, sandboxed exec) are
shared lineage; Hermes is the more complete, enterprise-aimed runtime. You're
already on the right tool — the work is hardening and operationalizing it, not
switching frameworks.

---

## 2. What you have today (honest assessment)

Your install is feature-rich but **operated lightly**: a single `no_agent` cron
job (`wiki_vault_autosync.sh`, daily 15:50) that commits/pushes your Obsidian
vault to GitHub. Almost every powerful Hermes primitive is installed but left at
defaults or disabled. That's the opportunity.

**Strong foundations already in place**
- Capable model routing: `gpt-5.5` via `openai-codex`, plus a local Ollama lane
  (`kimi-k2.6:cloud`, `qwen3.6:27b`).
- Full CLI toolset surface available: `browser, clarify, code_execution,
  cronjob, delegation, file, image_gen, kanban, memory, messaging, moa, rl,
  session_search, skills, terminal, todo, tts, vision, web, x_search`.
- Subagent delegation enabled (`orchestrator_enabled: true`).
- Memory + user profile on (`retaindb`).
- Tirith policy engine present (`bin/tirith`) and enabled.
- Curator on (weekly skill hygiene).
- A large bundled skill library (research, devops, github, software-development,
  red-teaming, mlops, autonomous-ai-agents, etc.).

**Gaps that block enterprise trust** (each addressed in §4 and §6)

| Area | Current state | Why it's a problem |
|---|---|---|
| Execution isolation | `terminal.backend: local` | Agents run shell **on your host**. Docker images are configured but unused. No blast-radius containment. |
| Policy fail mode | `security.tirith_fail_open: true` | If the policy engine errors, actions are **allowed**. Enterprise must fail **closed**. |
| Dangerous allowlist | `command_allowlist: [execute_code, "delete in root path"]` | Auto-approves code execution and a delete-in-root pattern — the opposite of least privilege. |
| Lazy installs | `security.allow_lazy_installs: true` | Agents can install arbitrary packages unsupervised (supply-chain risk). |
| No rollback | `checkpoints.enabled: false` | No automatic snapshot before destructive file/terminal ops. |
| Single point of failure | `fallback_providers: []`, `fallback_model` commented out | One provider outage stalls the whole fleet. |
| Secrets in plaintext | keys in `~/.hermes/.env` + `auth.json`; `secrets.bitwarden.enabled: false` | No vault-backed secret injection or rotation. |
| No observability | only `disk-cleanup` plugin enabled; Langfuse/NeMo off | No traces, no audit dashboard, no cost analytics for unattended runs. |
| Coarse identity | one global profile/persona | No per-role agents, no scoped permissions, no tenant separation. |
| Persona drift | `SOUL.md` = Alfred; `USER.md` memory says you now want Rick Sanchez | Memory and persona file disagree — worth reconciling. |

> Note: you also have a separate `agentic-loop-harness` (FastAPI + React/Vite,
> per your Hermes memory). Decide early whether that becomes the **dashboard /
> control plane** in front of Hermes (recommended) or stays a parallel
> experiment — don't maintain two orchestrators.

---

## 3. Target architecture (the layers a company would trust)

Think of it as seven layers. Hermes already provides a real primitive for each;
the job is to turn them on and wire them together.

```
┌───────────────────────────────────────────────────────────────┐
│ 7. Control plane / dashboard  (your agentic-loop-harness UI)    │
├───────────────────────────────────────────────────────────────┤
│ 6. Observability & audit   (Langfuse / NeMo ATOF+ATIF, kanban   │
│                             lifecycle = tamper-evident audit)   │
├───────────────────────────────────────────────────────────────┤
│ 5. Orchestration   (Kanban worker lanes + delegation +          │
│                     maker–checker subagents + /goal loops)      │
├───────────────────────────────────────────────────────────────┤
│ 4. Memory   (session → project → org tiers; retaindb +          │
│              skills as durable procedures)                      │
├───────────────────────────────────────────────────────────────┤
│ 3. Policy & approval   (Tirith fail-closed, allowlists,         │
│                         approval tiers, checkpoints/rollback)   │
├───────────────────────────────────────────────────────────────┤
│ 2. Identity & access (Agent IAM)  (profiles = roles, scoped     │
│                        toolsets, credential pools, tenants)     │
├───────────────────────────────────────────────────────────────┤
│ 1. Execution isolation   (Docker/sandbox per agent, worktrees)  │
└───────────────────────────────────────────────────────────────┘
```

### Layer 1 — Execution isolation
Move off `terminal.backend: local`. Hermes supports `local, docker, ssh, modal,
daytona, singularity` backends (`tools/environments/`). For any agent that runs
code or shell, use **docker** (you already declare
`nikolaik/python-nodejs:python3.11-nodejs20`, 1 CPU / 5 GB / 50 GB disk). Code
agents additionally get **git worktrees** so parallel work never collides — the
same isolation pattern your loops note recommends. Keep a `local` lane only for
trusted, read-only utility jobs.

### Layer 2 — Identity & access (Agent IAM)
This is the heart of "enterprise trust." Hermes gives you the primitives to
build real RBAC for agents:

- **Profiles = roles.** Create a Hermes **profile** per role
  (`researcher`, `code-reviewer`, `gtm-scout`, `inbox-triage`, `oncall`). Each
  profile is its own `HERMES_PROFILE` with isolated home, memory, sessions, and
  — critically — its **own toolset whitelist** (`disabled_toolsets` /
  `platform_toolsets`). A research agent gets `web, x_search, file(read),
  memory`; it does **not** get `terminal` or `messaging-send`.
- **Least-privilege toolsets.** Grant the minimum surface per role. This is the
  agent equivalent of an IAM policy document.
- **Credential pools = scoped, rotating keys.** `hermes auth add <provider>`
  registers multiple keys per provider with rotation strategies
  (`round_robin / least_used / fill_first / random`) and automatic failover on
  429/402/401. Scope keys per role so a GTM agent's key can't run your code
  infra, and revoke per-role without touching everything else.
- **Tenants.** Kanban already threads a `HERMES_TENANT` namespace through every
  worker. Use it to separate "personal", "nexus3-internal", and any
  client/customer work so memory and workspaces never bleed across boundaries.
- **Approval tiers per role.** High-trust roles (read-only research) can run
  unattended; high-blast-radius roles (deploy, send, delete, spend) require
  human approval regardless of how confident the agent is.

### Layer 3 — Policy & approval
- **Tirith fail-closed.** Flip `tirith_fail_open: false`. If the policy engine
  can't evaluate an action, deny it. (Enterprise non-negotiable.)
- **Real allowlist hygiene.** Remove `execute_code` and `delete in root path`
  from `command_allowlist`. Auto-approve only genuinely safe, idempotent
  commands; everything destructive flows through approval.
- **No lazy installs in unattended lanes.** Set `allow_lazy_installs: false`
  for cron/kanban roles; pre-bake dependencies into the docker image instead.
- **Checkpoints on.** `checkpoints.enabled: true` gives shadow-git snapshots
  before `write_file`, `patch`, and destructive shell (`rm`, `mv`, `sed -i`,
  `git reset/clean/checkout`, redirects), with `/rollback` recovery.
- **Approval boundaries** (the irreversible set): merge, deploy, send
  message/email, publish, delete, spend, dependency upgrade, auth/permission
  change, infra change. These always need a human — matches your
  "monitor first" stance.

### Layer 4 — Memory (tiered)
- **Session memory** — within a run (compression engine already on).
- **Project memory** — `AGENTS.md` / skills per workspace encode conventions,
  build/test commands, and past incidents so each run doesn't re-derive them.
- **Org memory** — `retaindb` (`~/.hermes/memories/MEMORY.md` + `USER.md`),
  optionally upgraded to a memory-provider plugin (honcho / mem0 / supermemory)
  when you want semantic recall across agents.
- **The learning loop** (from your loops note): *fail → investigate → verify →
  distill into a reusable rule/skill → consult later.* A memory store that's
  never read is just an audit trail. Your Curator (weekly) is the janitor that
  keeps this from rotting.

### Layer 5 — Orchestration
- **Kanban as the task kernel.** It owns the canonical lifecycle
  (`ready → running → blocked/done/archived`) and a tamper-evident event trail.
  `auto_decompose` is already on — big goals become cards automatically.
- **Worker lanes = role executors.** Route a card's `assignee` to a profile lane
  (`hermes -p <role> chat -q <prompt>` in a pinned workspace) or an external CLI
  lane (Codex / Claude Code / OpenCode wrapped as a worker).
- **Maker–checker by default.** One subagent implements; a **separate
  fresh-context** subagent verifies against spec/tests/security rubric. This is
  the single highest-leverage reliability pattern — never let the maker grade
  itself.
- **`/goal` loops** for "keep going until done" tasks (Hermes' Ralph-loop take):
  a lightweight judge model checks each turn against a measurable end state and
  auto-continues within a turn budget. Use for "fix every lint error and get CI
  green," not for open-ended "make it better."

### Layer 6 — Observability & audit
- Enable **Langfuse** (`hermes plugins enable observability/langfuse`) for
  per-turn traces, tool calls, token/cost analytics — the dashboard a manager
  asks for.
- For deeper harness analysis, **NeMo Relay** emits **ATOF** (JSONL event
  stream) and **ATIF** (replayable trajectories incl. subagent embedding) — good
  for evals and regression replay of agent behavior.
- The **kanban event log** is your audit of record: who/what/when for every card
  transition. Pair with `redact_secrets: true` (already on) and sanitized logs.

### Layer 7 — Control plane
Put a single pane of glass in front of it — ideally your existing
`agentic-loop-harness` dashboard — showing: live job status
(active/queued/done/failed, per your stated monitoring preference), spend, the
triage inbox, and approval queue. This is where "monitor first" actually lives.

---

## 4. Reliability & scale
- **Cross-provider fallback:** uncomment `fallback_model` (e.g. an OpenRouter or
  Anthropic route) so a primary outage doesn't halt the fleet. Pools handle
  same-provider key exhaustion; fallback handles whole-provider failure.
- **Budgets everywhere:** per-job token/time/iteration caps
  (`agent.max_turns`, `goals.max_turns`, `delegation.child_timeout_seconds`,
  kanban `failure_limit`, `dispatch_stale_timeout_seconds`). The success metric
  is **cost per accepted change**, not tasks attempted.
- **Concurrency:** raise `delegation.max_concurrent_children` and consider
  `max_spawn_depth > 1` only once isolation + budgets are proven.
- **Deployment path:** local now → a small always-on VPS or container host for
  the gateway + scheduler → optional AWS Bedrock provider (already scaffolded in
  config) for compliance-friendly model access. Keep state (`state.db`,
  `kanban.db`, `retaindb`) backed up; you already snapshot
  (`state-snapshots/`).

---

## 5. Config hardening checklist (apply to `~/.hermes/config.yaml`)

Do these in order; each is low-risk and reversible.

| # | Setting | Current → Recommended | Rationale |
|---|---|---|---|
| 1 | `security.tirith_fail_open` | `true` → **`false`** | Fail closed. |
| 2 | `command_allowlist` | `[execute_code, "delete in root path"]` → **`[]`** (or only safe read commands) | Least privilege. |
| 3 | `security.allow_lazy_installs` | `true` → **`false`** for unattended roles | Supply-chain safety. |
| 4 | `checkpoints.enabled` | `false` → **`true`** | Rollback safety net. |
| 5 | `terminal.backend` | `local` → **`docker`** for code/exec roles | Isolation. |
| 6 | `fallback_model` | commented → **configured** | No single point of failure. |
| 7 | `secrets.bitwarden.enabled` | `false` → **`true`** (or another vault) | Stop plaintext keys. |
| 8 | Observability | none → **enable `observability/langfuse`** | Audit + cost. |
| 9 | `approvals.destructive_slash_confirm` | `false` → **`true`** | Confirm destructive slash commands. |
| 10 | Profiles | single → **create role profiles** (`researcher`, `code-reviewer`, `gtm-scout`, `inbox-triage`, `oncall`) with scoped toolsets | Agent IAM. |
| 11 | Persona drift | reconcile `SOUL.md` (Alfred) vs `USER.md` memory (Rick Sanchez) | Pick one; avoid contradictory steering. |

---

## 6. The agent fleet — concrete standing agents (monitor-first)

Each agent below is written as a **job description**: trigger · body · verifier ·
escalation · budget · safety. All start **read-only / draft-only** and graduate
to action only after you trust the output. Implement as Hermes cron jobs (with
`cron_mode: deny` keeping destructive actions out of unattended runs), promoting
recurring wins into `SKILL.md` files.

### A. Coding & dev loops
1. **CI / repo sentinel** — *Every weekday 07:00:* pull GitHub notifications +
   CI status across your repos; summarize only what blocks a deadline or breaks
   a build. Verifier: build/test status is deterministic. Escalation: ping only
   on red. Budget: 1 pass, capped tokens. Safety: read-only.
2. **Nightly code reviewer** — *Nightly:* maker–checker review of the day's
   diffs for leaked secrets, debug logs, obvious regressions. Output: a markdown
   report + draft PR comments (not posted). Graduates to: auto-posting
   review comments once trusted; merges always human.
3. **PR babysitter (`/goal`)** — *On demand:* "get PR #N green" — iterate on
   failing tests/lint within a turn budget, in a docker worktree. Human approves
   merge.

### B. Research & knowledge (extends your existing wiki engine)
4. **Auto-ingest pipeline** — your `USER.md` already says you want *every link
   you send from any platform* extracted → researched → added to the Obsidian
   wiki (recipes get full ingredients+steps). Formalize it: a messaging-triggered
   job that runs the `HERMES_WIKI_SYSTEM_PROMPT` ingest standard, dedupes against
   canonical notes, updates `index.md`/`log.md`, and commits via your existing
   autosync. Verifier: schema/taxonomy check + dedupe. Escalation: silent;
   surface a weekly digest.
5. **Friday research digest** — *Fri 16:00:* dedupe the week's captured sources
   into one short brief with "why now." Read-only.
6. **Competitor / changelog watch** — *Daily:* monitor specific competitor
   release notes + arXiv/GitHub trends in your domains; only escalate genuinely
   new signals. Feeds BuilderPulse-style idea generation.

### C. Business / GTM & market-finding
7. **Market-radar / opportunity scout** — *Daily:* scan defined public signals
   (launches, dev tooling, funding, pain-point threads) and produce one
   buildable AI-product idea with a why-now thesis (mirrors your
   `BuilderPulse` note). Read-only → draft.
8. **Name / brand radar** — *Daily:* surface mentions of you / nexus3 / your
   products across noisy public channels; escalate only high-signal mentions.
9. **Outreach drafter** — *On account movement:* draft trigger-specific outreach
   (your `ai-gtm-brain-claude-code` workflow). **Draft-only, always** — sending
   is an approval-gated boundary.

### D. Comms & inbox triage
10. **Cross-channel triage** — *Hourly (work hours):* watch Telegram / Slack /
    Discord / email from one gateway; escalate only deadlines, waiting people,
    or money. Everything else → a digest. Read-only first.
11. **Reply drafter** — draft routine replies; **human approval before send**.
12. **On-call pre-diagnosis** — *On alert:* gather logs, form a hypothesis, and
    attach it *before* paging you. Read-only diagnosis.

---

## 7. Phased rollout (matches "monitor first, expand later")

**Phase 0 — Harden (week 1).** Apply the §5 checklist. Reconcile persona.
Decide control-plane (fold `agentic-loop-harness` in or retire it). No new
autonomy yet.

**Phase 1 — Observe (weeks 1–2).** Enable Langfuse. Stand up agents **A1, B5,
C7, D10 in read-only**. Watch traces, output quality, and spend daily. Goal:
trust + a baseline cost-per-useful-output.

**Phase 2 — Roles & isolation (weeks 2–4).** Create the role profiles with
scoped toolsets, docker execution, credential pools, tenants. Add maker–checker
to the code reviewer (A2). Introduce the kanban board as task kernel.

**Phase 3 — Graduated action (week 4+).** Promote individual agents from
draft-only to acting, one boundary at a time, each behind approval for
irreversible steps. Convert every repeated win into a `SKILL.md`. Add `/goal`
loops where verification is automated (A3).

**Phase 4 — Scale (ongoing).** Raise concurrency/depth, add fallback routing and
Bedrock if needed, move the gateway to an always-on host, and keep the Curator +
weekly digests pruning memory and skills so the system learns instead of just
logging.

---

## 8. Risks & guardrails (carry these into every loop)
- **Confident mistakes at scale** — independent verification before any action;
  maker ≠ checker.
- **Runaway spend** — hard token/dollar/iteration caps per job; review week-one
  spend.
- **Over-notification** — every job needs an explicit "only notify me when…"
  clause; default to silence + digest.
- **Privilege creep** — re-audit toolset scopes, write/deploy perms, and
  connector scopes periodically.
- **Skills as injection vectors** — never auto-install community skills into
  unattended loops; audit source + permissions first.
- **Comprehension debt / cognitive surrender** — keep reading diffs and
  spot-checking gates; the loop should increase leverage while you still own
  what ships.
- **Credential leakage** — vault-backed secrets, sanitized logs, per-role keys.

---

## 9. Sources
- `workflows/agentic-coding-loops.md` — loop engineering, `/goal`, maker–checker, memory loops.
- `workflows/hermes-standing-agent-prompts.md` — trigger/body/escalation job-description model.
- `workflows/claude-dynamic-workflows.md`, `projects/evo-autoresearch-workflow-loops.md` — orchestration patterns.
- Local Hermes repo docs (`~/.hermes/hermes-agent/website/docs`): `guides/migrate-from-openclaw.md`, `guides/delegation-patterns.md`, `user-guide/features/{credential-pools,kanban-worker-lanes,goals,tool-gateway}.md`, `user-guide/checkpoints-and-rollback.md`, `plugins/observability/{langfuse,nemo_relay}`.
- Your live config (`~/.hermes/config.yaml`), `SOUL.md`, `memories/{MEMORY,USER}.md`, `cron/jobs.json`, `scripts/wiki_vault_autosync.sh`.
