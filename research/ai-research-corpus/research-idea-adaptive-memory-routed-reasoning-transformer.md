---
title: Adaptive Memory-Routed Reasoning Transformer
type: research-idea
status: sharpened-plan
created: 2026-06-15
updated: 2026-06-16
source: local-wiki-synthesis
corpus: curated_modern_ai_research_papers.md
research_question: Can a verifier-gated, adaptive memory-routing architecture improve long-context reasoning and agent performance per token/FLOP compared with dense Transformers, long-context attention, static RAG, and memory-agent baselines?
---

# Adaptive Memory-Routed Reasoning Transformer

## One-line thesis

AMRRT tests whether a model or model-adjacent runtime can learn a cost-aware policy for when to use parametric knowledge, local context, compressed KV memory, external retrieval, tools, high-compute reasoning, and memory writeback, instead of blindly stuffing context or always invoking agent machinery.

## Precise hypothesis

A small open LLM augmented with a learned, verifier-gated memory router will outperform static long-context prompting and static RAG on dispersed-evidence reasoning and repeated-episode agent tasks at equal or lower token budget.

More precisely:

1. On long-context and multi-hop tasks with distractors, AMRRT will improve answer accuracy or pass rate by at least 5 absolute points over static RAG while using no more than 70 percent of its retrieved/input tokens.
2. On repeated-episode agent tasks, AMRRT will improve second-and-later episode success over a no-memory or append-only-memory agent by at least 10 relative percent.
3. AMRRT will keep memory contamination below 2 percent of accepted writes, measured as false, unverifiable, stale, or task-misgeneralized memory items.
4. If AMRRT only wins by using materially more total latency, tool calls, retrieved tokens, or expert FLOPs than the best static baseline, the hypothesis is not supported.

The falsifiable core is not “memory helps.” The test is whether learned timing and selective memory/compute routing beat strong simple baselines under a fixed cost budget.

## Research question

Can a language model learn when to use internal parameters, local sequence state, compressed KV memory, external retrieval memory, and tool/agent memory, instead of receiving all context up front, and thereby improve long-context reasoning, agent reliability, and inference cost?

## Gap found in the wiki corpus

The curated corpus already covers the ingredients separately:

- Transformers and scaling: `Attention Is All You Need`, GPT-3, PaLM, Chinchilla, LLaMA, Qwen, DeepSeek.
- Efficient and sparse architectures: GShard, DeepSeekMoE/MLA, DBRX, MiniMax Lightning Attention, Hunyuan Mamba-Transformer hybrids, Mamba, RWKV.
- Retrieval and memory: RAG, RETRO, Memorizing Transformers, BGE, HyDE, MemGPT, Lost in the Middle.
- Agent loops and tool use: ReAct, Toolformer, Gorilla, ToolBench, AgentBench, Reflexion, Voyager, SWE-agent, AutoGen.
- Test-time reasoning and loop engineering: Chain-of-Thought, self-consistency, Tree of Thoughts, Self-Refine, DeepSeek-R1, Kimi k1.5, MiniMax-M1.
- Evaluation harnesses: HELM, LongBench, SWE-bench, OpenCompass, Chatbot Arena, BIG-bench.

The underworked integration is a single testable system that jointly learns:

1. when retrieval is needed,
2. which memory surface to query,
3. how much evidence to admit,
4. when to activate high-compute reasoning,
5. when to call tools,
6. when to write persistent memory,
7. and when to stop.

Most RAG systems retrieve before the model has decomposed the problem. Most memory agents use harness rules. Most MoE systems route compute internally but do not route external context, tools, or writeback. AMRRT’s gap is to make routing across memory and compute a trained policy with explicit cost and contamination penalties.

## Proposed architecture

AMRRT has five components.

### 1. Base reasoning stream

Start with a small open instruction model, such as Qwen2.5/Qwen3 0.5B-1.5B, OLMo-scale models, or another reproducible 0.5B-3B base. The first prototype should not require pretraining a new foundation model.

Two implementation levels are possible:

- Level 1: harness-level router around a frozen or LoRA-tuned model.
- Level 2: internal router tokens/adapters inserted at selected layers or reasoning steps.

The Level 1 version is the first experiment because it can be built and falsified quickly.

### 2. Learned memory router

At each reasoning step or segment boundary, the router chooses one action from a bounded action set:

- `answer_from_weights`
- `use_local_context`
- `retrieve_vector_memory`
- `retrieve_graph_memory`
- `retrieve_compressed_episode`
- `call_tool`
- `activate_reasoning_expert`
- `write_candidate_memory`
- `stop`

Router input features:

- current question and partial reasoning state,
- uncertainty score or entropy proxy,
- retrieval score distribution from a cheap prefetch,
- contradiction signal between candidates,
- task type embedding,
- remaining token/tool/FLOP budget,
- prior episode success/failure summaries.

Router training objective:

- supervised cross-entropy from generated oracle traces,
- plus cost penalties for tokens, calls, and expert activation,
- plus negative reward for irrelevant retrieval or contaminated writes,
- optionally policy optimization after the supervised warm start.

### 3. Episodic memory bank

Memory entries are compact records, not transcript dumps.

Suggested schema:

- `memory_id`
- `task_family`
- `claim_or_skill`
- `supporting_source_ids`
- `episode_context_hash`
- `outcome`
- `verifier_score`
- `expiry_or_scope`
- `embedding`
- `graph_links`, if available

The key experimental constraint: dumping the entire memory is disallowed or penalized. The model must retrieve sparse, useful memories.

### 4. Sparse high-compute reasoning expert path

The high-compute path can be implemented initially without custom kernels:

- run more deliberation steps,
- sample multiple chains and vote,
- call a larger teacher model for distilled traces during training only,
- or use a LoRA adapter specialized for decomposition/tool planning.

The router must pay a cost for this path. The point is not simply “think longer”; it is “think longer only when the policy predicts that extra compute is worth it.”

### 5. Verifier-gated memory writeback

After an episode, AMRRT proposes candidate memories. A verifier accepts, rejects, scopes, or expires them.

Verifier signals:

- exact-match or unit-test outcome where available,
- citation/evidence support for factual claims,
- contradiction check against existing memory,
- task-generalization check: does this memory apply beyond the current episode?

Accepted writes become future retrieval candidates. Rejected writes are logged for router training as negative examples.

## Minimal prototype design

Build the first prototype as a harness-level system before modifying model internals.

### Prototype loop

1. Receive task/question and budget.
2. Generate an initial compact problem state.
3. Router chooses an action.
4. If retrieval/tool/expert is selected, execute only that action and append a bounded observation.
5. Model updates the reasoning state.
6. Repeat until `stop` or budget exhaustion.
7. Generate final answer with explicit source/memory provenance.
8. Verifier scores answer and candidate memory writes.
9. Accepted memory writes are stored; rejected writes become negative training examples.

### First build target

A small Python harness around an open model is sufficient:

- model: Qwen2.5-1.5B-Instruct, Qwen3 small, OLMo, or similar,
- adaptation: LoRA for router-conditioned behavior,
- retrieval: BGE embeddings plus a local vector DB or FAISS/Qdrant,
- graph memory: optional in phase 1; use only if the corpus has reliable graph edges,
- verifier: deterministic labels where possible, LLM judge only for non-deterministic tasks,
- routing model: small encoder or classifier trained on trace features.

Do not start by pretraining a new Transformer. The first question is whether the adaptive policy produces measurable gains at all.

## Training and evaluation setup

### Data generation

Create traces for each training task with multiple policies:

- no retrieval,
- static top-k retrieval,
- retrieve-after-decomposition,
- oracle retrieval using known supporting facts,
- tool call when evidence is absent,
- expert reasoning only on hard examples.

Label the best action sequence by answer correctness under cost constraints. This creates router supervision.

### Training stages

1. Freeze the base LLM and train only the router on oracle and heuristic traces.
2. Add LoRA adapters so the model learns to consume router observations and produce compact reasoning states.
3. Train the memory-write verifier or use deterministic verifiers where benchmark labels permit.
4. Fine-tune with a cost-aware reward:
   - positive: correct answer, passed test, verified tool result, useful memory reuse;
   - negative: irrelevant retrieval, excessive tokens, unnecessary tool calls, contaminated memory writes.
5. Keep a held-out task-family split to test whether the router generalizes rather than memorizes benchmark quirks.

### Evaluation protocol

For every benchmark, report both quality and cost:

- accuracy, F1, exact match, pass rate, or benchmark-native score,
- retrieved-token precision,
- evidence recall,
- total input/output tokens,
- number of retrievals,
- number of tool calls,
- latency,
- activated expert FLOPs or proxy cost,
- memory contamination rate,
- repeated-episode improvement,
- abstention/stop correctness.

The headline table should be budget-normalized: same max tokens, same max tool calls, same retrieval corpus, same base model family.

## Baselines

Use strong simple baselines, not strawmen.

1. Base decoder-only model with ordinary prompting.
2. Long-context prompting with full relevant context packed up front.
3. Static RAG with fixed top-k retrieval before generation.
4. Query-rewritten RAG or HyDE-style retrieval.
5. ReAct with tools but no persistent memory.
6. Reflexion-style append-only verbal memory.
7. MemGPT-style memory manager or equivalent harness-level memory system.
8. Long-context efficient architecture baseline where available, such as Mamba/Transformer hybrid or linear-attention model.
9. Oracle retrieval upper bound using known supporting passages.
10. Oracle router upper bound to separate router-learning failure from architecture ceiling.

## Datasets and benchmarks

### Long-context and dispersed-evidence reasoning

- LongBench
- Lost-in-the-Middle key-value and multi-document stress tests
- HotpotQA or 2WikiMultiHopQA with injected distractors
- NarrativeQA or Qasper-style long-document QA, if compute permits

### Retrieval precision and memory routing

- Synthetic precision-aware memory benchmark:
  - each task has a large memory bank,
  - only 1-3 memories are useful,
  - irrelevant memories are semantically similar distractors,
  - score penalizes retrieving or reading the full memory store.
- RAG benchmarks with known supporting passages.

### Tool and agent routing

- ToolBench / ToolLLM-style API selection tasks
- AgentBench tasks with bounded tool budgets
- ToolQA for tool-required question answering

### Coding and repeated-episode memory

- RepoBench-style repository context tasks
- SWE-bench Lite or a smaller issue-fix subset if compute permits
- HumanEval-style coding tasks with repeated library/API lessons injected across episodes
- A custom “recurring bug memory” benchmark where the same project conventions matter across tasks

### Safety/contamination benchmark

Create adversarial memory episodes:

- false successful memories,
- outdated API memories,
- task-local tricks that should not generalize,
- contradictory memories with different provenance scores.

The goal is to measure whether verifier-gated writeback prevents future harm.

## Ablations

Run these before making large claims:

1. No adaptive retrieval: fixed top-k retrieval only.
2. No verifier gate: write all proposed memories.
3. No persistent memory: retrieve only from task context/corpus.
4. No compressed episode memory: vector memory only.
5. No graph memory: vector retrieval only.
6. No reasoning expert: router can retrieve but cannot activate extra compute.
7. Always expert: test whether simple more-compute beats routing.
8. Random router with same action budget.
9. Heuristic router with rules instead of learned policy.
10. Oracle router upper bound.
11. Different router granularity: per question, per paragraph, per reasoning step, per generated tool/action token.
12. Different cost penalties to test quality/cost frontier.

## Failure modes

1. Router under-retrieves and confidently answers from weak parametric memory.
2. Router over-retrieves, recreating noisy RAG with extra overhead.
3. Routing supervision leaks benchmark labels or teaches benchmark-specific shortcuts.
4. Verifier accepts plausible but false memories.
5. Useful memories are too specific and fail to transfer.
6. Negative memories suppress valid future strategies because scope/expiry is wrong.
7. The high-compute expert dominates gains, making memory routing irrelevant.
8. Tool latency erases token savings.
9. Retrieval embeddings fail on compositional or code-specific queries.
10. The system improves average scores but worsens tail risk through rare contaminated writes.
11. Graph memory adds brittle edges and hurts more than vector retrieval.
12. Evaluation accidentally rewards abstention or minimal retrieval rather than solving the task.

Rollback plan for each failure:

- If router quality fails, compare to oracle router and heuristic router to determine whether the policy is learnable.
- If verifier quality fails, restrict memory writes to deterministic-outcome tasks first.
- If latency fails, disable tool calls and expert path, then retest retrieval-only routing.
- If graph memory fails, remove graph memory from phase 1 and keep it as a later ablation.

## Practical implementation roadmap

### Phase 0: spec and harness skeleton, 2-3 days

- Define action schema, trace schema, memory schema, and cost accounting.
- Build a deterministic evaluator wrapper.
- Implement static baselines first.
- Freeze benchmark splits and budgets before tuning.

Success gate: baseline scores and cost metrics reproduce consistently.

### Phase 1: harness-level AMRRT, 1-2 weeks

- Implement router as a small classifier over task/reasoning/retrieval features.
- Use frozen LLM plus prompt protocol for reasoning-state updates.
- Add vector memory and candidate memory writeback.
- Use deterministic verification where possible.

Success gate: router beats random and heuristic policies on held-out traces.

### Phase 2: supervised router and LoRA adaptation, 2-4 weeks

- Generate oracle/near-oracle traces with static RAG, decomposed RAG, and oracle retrieval.
- Train router on action labels with cost weights.
- LoRA-tune the base model to consume retrieved observations and produce concise state.
- Evaluate against static RAG and ReAct under equal budgets.

Success gate: at least one benchmark family shows quality gain at lower retrieved-token cost.

### Phase 3: verifier-gated memory, 2-3 weeks

- Add candidate memory extraction.
- Train or prompt a verifier with deterministic checks where possible.
- Add memory scope, expiry, provenance, and contradiction checks.
- Run repeated-episode benchmarks.

Success gate: memory reuse improves later episodes without contamination exceeding the threshold.

### Phase 4: reasoning expert and internalization, 4-8 weeks

- Add sparse high-compute path as a budgeted action.
- Compare always-expert, never-expert, and routed-expert variants.
- If harness-level results hold, test internal router tokens or adapter modules at selected layers.

Success gate: routed expert improves quality/cost frontier over always-expert and never-expert.

### Phase 5: paper-quality evaluation, 2-4 weeks

- Run all baselines and ablations.
- Report statistical confidence across seeds/task subsets.
- Publish failure cases, contamination audit, and cost-normalized results.

Success gate: AMRRT wins on the claimed task classes and loses honestly elsewhere.

## Why this could outperform existing architectures

AMRRT could outperform static RAG, long-context prompting, and ordinary memory agents because it attacks their shared inefficiency: they decide context policy too early or too crudely.

- Long-context models make evidence available but do not guarantee the model uses the right evidence, as shown by Lost in the Middle.
- Static RAG retrieves before the reasoning state has exposed the real subproblem, causing distractor evidence and wasted tokens.
- Agent memory systems often store and retrieve by hand-written rules, so memory quality depends on harness design rather than a learned cost-aware policy.
- MoE and adaptive-compute models route internal compute, but usually ignore external memory, tool calls, and writeback quality.

AMRRT’s advantage is conditional allocation: cheap local reasoning for easy cases, precise retrieval for evidence gaps, tools for missing external state, high-compute reasoning for hard decomposition, and verifier-gated writes only when a lesson is worth keeping. If this policy is learnable, it should move the quality/cost frontier rather than merely increasing raw capability through more context or more compute.

## Related work and novelty boundary

This is not an absolute novelty claim. The corpus and quick live arXiv check show many close components:

- RAG: https://arxiv.org/abs/2005.11401
- RETRO: https://arxiv.org/abs/2112.04426
- Memorizing Transformers: https://arxiv.org/abs/2203.08913
- Lost in the Middle: https://arxiv.org/abs/2307.03172
- MemGPT: https://arxiv.org/abs/2310.08560
- ReAct: https://arxiv.org/abs/2210.03629
- Reflexion: https://arxiv.org/abs/2303.11366
- Voyager: https://arxiv.org/abs/2305.16291
- Toolformer: https://arxiv.org/abs/2302.04761
- ToolBench: https://arxiv.org/abs/2307.16789
- AgentBench: https://arxiv.org/abs/2308.03688
- LongBench: https://arxiv.org/abs/2308.14508
- SWE-bench: https://arxiv.org/abs/2310.06770
- DeepSeek-V3: https://arxiv.org/abs/2412.19437
- MiniMax-01: https://arxiv.org/abs/2501.08313
- Mamba: https://arxiv.org/abs/2312.00752
- vLLM/PagedAttention: https://arxiv.org/abs/2309.06180

Live arXiv checks on 2026-06-16 also surfaced related work around:

- adaptive reasoning suppression for efficient reasoning models,
- reflective memory retrieval for LLM agents,
- graph memory for LLM agents,
- memory-guided reflective retrieval for dialogue agents,
- KV-cache eviction and memory management,
- adaptive or regime-conditional retrieval for multi-hop QA.

The narrower novelty claim is the combined, testable system: learned cost-aware routing across retrieval timing, memory type, tool use, high-compute reasoning, and verifier-gated persistent writeback, evaluated against static RAG, long-context, ReAct/Reflexion/MemGPT-style memory baselines, and oracle routing under fixed budgets.

## Minimum publishable result

The idea is worth continuing only if the harness-level prototype shows at least one of these:

1. better accuracy than static RAG at materially lower retrieved-token cost on long-context or multi-hop tasks,
2. better repeated-episode success than append-only memory agents with low contamination,
3. better tool-routing success/cost than ReAct-style prompting,
4. a meaningful gap between learned router and heuristic router, showing that routing policy learning matters.

If none of these appear, AMRRT should be reframed as an evaluation/harness result rather than a new architecture.
