---
title: Memory-Native Transformer
type: research-idea
status: draft-architecture
created: 2026-06-16
source: local-wiki-synthesis
corpus: curated_modern_ai_research_papers.md
parent_idea: research-idea-adaptive-memory-routed-reasoning-transformer.md
scope: LLM architecture and memory only; no agents, tools, or external action loops
research_question: Can an LLM architecture with learned hierarchical read/write/consolidation memory outperform long-context Transformers and static retrieval-augmented LMs on long-range reasoning and repeated-information tasks under the same compute/token budget?
---

# Memory-Native Transformer

## Current focus

This note narrows the earlier AMRRT idea to only LLM architecture and memory. It intentionally removes agent loops, tool calls, API use, and external action policies.

The target is a new LLM architecture plus memory system, not an agent harness.

Working name: Memory-Native Transformer, or MNT.

## Core thesis

Current LLMs mostly treat memory in one of three incomplete ways:

1. Parametric memory: facts and procedures are stored in weights after training.
2. Context memory: tokens are placed in the context window and attended to directly.
3. Retrieval memory: documents are fetched before generation and inserted as text.

MNT proposes a fourth design: memory as a first-class architectural state. The model learns to form, retrieve, update, consolidate, and forget compact memory records while doing language modeling.

The key change is that long context is not only longer attention. It becomes a learned memory hierarchy.

## Precise hypothesis

A decoder-only LLM with a learned hierarchical memory system can outperform a same-size long-context Transformer and a static RAG baseline on long-range dependency, repeated-fact, and multi-hop language tasks at equal or lower inference token budget.

Falsifiable version:

1. On long-context tasks, MNT should beat a same-parameter Transformer with full-context attention when both are constrained to the same effective FLOP budget.
2. On repeated-information tasks, MNT should improve after seeing repeated episodes without requiring weight updates.
3. On retrieval-heavy tasks, MNT should retrieve fewer memory tokens than static RAG while preserving or improving answer accuracy.
4. If gains disappear when memory write/consolidation losses are removed, the architecture is doing real memory work. If gains remain, the memory system is probably ornamental.

## Proposed architecture

MNT is a decoder-only Transformer variant with four memory surfaces and a memory controller.

### 1. Parametric memory

This is the ordinary model weights. It stores stable language knowledge, reasoning circuits, syntax, world regularities, and learned procedures.

No change here except that training should reduce pressure to memorize ephemeral facts in weights.

### 2. Working memory: local tokens and KV cache

This is the normal short-horizon context stream.

Purpose:

- immediate syntax,
- local coherence,
- current reasoning state,
- recent entities and constraints.

Implementation:

- sliding-window or paged attention over recent tokens,
- optional compressed KV summaries for older local spans,
- attention sink or recurrent summary tokens for continuity.

This layer handles the easy case. The model should not consult persistent memory for every token.

### 3. Episodic memory: compressed event records

This is the new central component.

Instead of storing every old token, the model writes compact memory records after important spans.

Each episodic memory slot stores:

- key vector: what would make this memory relevant later,
- value vector: compressed content,
- source span pointer: token range or document span that produced it,
- entity/topic tags: learned or extracted lightweight labels,
- recency and frequency counters,
- confidence/usefulness score,
- decay/expiry value,
- contradiction links, if another memory conflicts.

Episodic memory is not a vector database bolted onto the model. It is a model-visible recurrent state with learned read/write behavior.

### 4. Semantic memory: consolidated prototypes

When similar episodic memories recur, the model consolidates them into stable semantic memory slots.

Example:

- episodic memories: “In this repo, tests use uv,” “This project runs pytest with uv,” “Python commands here use uv run.”
- semantic memory: “For this project, Python execution convention is uv run.”

Semantic memory is slower-changing than episodic memory. It should be more abstract, more compressed, and less tied to a single span.

Implementation options:

- prototype memory slots updated by exponential moving average,
- learned product-key memory,
- low-rank memory adapters selected by topic,
- differentiable key-value table with consolidation loss.

The first prototype should use explicit memory slots, not weight updates, because slot memory is easier to inspect and ablate.

### 5. Memory controller

At selected layers or segment boundaries, a small controller predicts memory operations:

- `read_working`
- `read_episodic`
- `read_semantic`
- `write_episodic`
- `consolidate_semantic`
- `decay_or_forget`
- `ignore_memory`

The controller sees:

- hidden state summary,
- uncertainty proxy,
- attention failure signal,
- distance from relevant prior evidence,
- memory retrieval score distribution,
- token/FLOP budget.

This controller is much narrower than AMRRT’s router. It does not choose tools, agents, or external actions. It only controls memory inside the LLM.

## Layer-level design

A practical block could look like this:

1. Token stream enters standard self-attention or sliding-window attention.
2. A small memory query head projects hidden states into memory-query vectors.
3. Top-k episodic and semantic memory slots are retrieved by differentiable or approximate nearest-neighbor lookup.
4. Retrieved memory vectors are injected through gated cross-attention.
5. A write head proposes new episodic memory slots at segment boundaries.
6. A consolidation head merges repeated or high-utility episodic slots into semantic slots.
7. A forget head decays low-utility, stale, or contradicted memories.

Block equation sketch:

```text
h_local = LocalTransformerBlock(x, local_kv)
q_mem = Wq_mem(pool(h_local))
M_e = topk(EpisodicMemory, q_mem)
M_s = topk(SemanticMemory, q_mem)
h_mem = CrossAttention(h_local, concat(M_e, M_s))
g = sigmoid(Wg[h_local; h_mem; budget_features])
h_out = h_local + g * h_mem
write_candidates = WriteHead(h_out at segment boundaries)
```

The gate is important. Without it, memory will become noisy context stuffing by another name.

## TurboQuant and DeepSeek MLA integration

Two additional mechanisms make MNT more technically plausible: quantized memory slots and latent multi-head attention.

### TurboQuant as memory-slot compression

TurboQuant, `TurboQuant: Online Vector Quantization with Near-optimal Distortion Rate`, is not an LLM architecture paper by itself. Its relevance is that MNT’s episodic and semantic memories are vectors, and a useful memory-native model will need to store many of them cheaply.

MNT can use TurboQuant-style online vector quantization for:

- episodic memory key vectors,
- episodic memory value vectors,
- semantic prototype vectors,
- older compressed KV summaries,
- approximate retrieval indexes over memory slots.

Why this matters:

- If memory slots are full precision, persistent memory grows too expensive.
- If memory slots are naively quantized, retrieval may fail because inner-product similarity is distorted.
- TurboQuant is directly relevant because it targets vector quantization with low distortion, including inner-product distortion, which is exactly what memory-slot retrieval depends on.

Practical design:

```text
raw_memory_value = WriteHead(h_segment)
quantized_value = TurboQuantLikeQuantizer(raw_memory_value, bits=b)
memory_slot.value_vector = quantized_value
memory_slot.quantization_error = estimate_distortion(raw_memory_value, quantized_value)
read_score -= distortion_penalty * memory_slot.quantization_error
```

The architecture should treat quantization error as part of memory confidence. A heavily compressed memory can still be useful, but the read gate should know it is less trustworthy.

### DeepSeek MLA as local-memory compression

DeepSeek-V2 and DeepSeek-V3 use Multi-head Latent Attention, or MLA, to reduce KV-cache cost by projecting key/value information into a compact latent representation while preserving multi-head attention behavior. This is highly relevant to MNT because the boundary between KV cache and episodic memory should not be a cliff.

MNT can adapt MLA in two places:

1. Working memory compression
   - Recent context uses normal or sliding attention.
   - Older local context is projected into MLA-style latent KV states.
   - Those latent states become candidates for episodic memory writes.

2. Memory read attention
   - Instead of retrieving raw text chunks, MNT retrieves compact memory slots.
   - Multiple memory-attention heads can specialize:
     - entity head,
     - temporal/order head,
     - causal/reasoning head,
     - contradiction head,
     - task/procedure head.
   - A latent shared representation keeps KV and memory bandwidth manageable.

A revised block sketch:

```text
h_local = LocalAttention(x, recent_kv)
latent_kv = MLACompress(older_kv)
q_mem_heads = MultiHeadMemoryQuery(h_local)
M_e = quantized_topk(EpisodicMemory, q_mem_heads)
M_s = quantized_topk(SemanticMemory, q_mem_heads)
h_latent = LatentCrossAttention(h_local, latent_kv)
h_mem = MultiHeadMemoryAttention(h_local, M_e, M_s)
g = sigmoid(Wg[h_local; h_latent; h_mem; budget_features])
h_out = h_local + g_latent * h_latent + g_mem * h_mem
```

### Updated architectural claim

The stronger MNT variant is not just “Transformer plus memory.” It is:

- MLA-style latent attention for efficient working-memory compression,
- TurboQuant-style vector quantization for cheap persistent memory slots,
- multi-head memory attention for specialized retrieval behavior,
- learned read/write/consolidate/forget gates to prevent memory spam.

This makes the idea more viable because it addresses the main engineering objection: persistent memory may help quality, but it can easily lose on cost. MLA and TurboQuant-style compression give MNT a plausible route to a better quality/cost frontier rather than a bigger, slower memory contraption wearing a lab coat.

## Memory system design

### Memory record format

```text
MemorySlot:
  slot_id
  memory_type: episodic | semantic
  key_vector
  value_vector
  source_pointer
  abstraction_level
  confidence
  utility_score
  recency
  frequency
  expiry
  contradiction_links
```

### Read policy

The read policy should prefer:

1. high relevance,
2. high confidence,
3. low redundancy,
4. high expected utility for the current prediction or reasoning step.

Scoring sketch:

```text
score(memory, state) =
  relevance(query, memory.key)
  + confidence_weight * memory.confidence
  + utility_weight * memory.utility_score
  + recency_weight * recency(memory)
  - redundancy_penalty
  - contradiction_penalty
  - token_cost_penalty
```

### Write policy

The model should write episodic memory only when a span contains information likely to matter later.

Write triggers:

- entity definition,
- rule or constraint,
- user/project preference,
- causal relation,
- exception/failure case,
- result of a long reasoning chain,
- repeated fact with stable support.

Write rejections:

- generic filler,
- unsupported speculation,
- transient scratchpad tokens,
- facts already stored with higher confidence,
- contradictions without enough evidence.

### Consolidation policy

Semantic consolidation happens when episodic memories cluster and repeatedly prove useful.

A memory should consolidate if:

- it appears across multiple contexts,
- it improves predictions or answers when retrieved,
- it has low contradiction rate,
- it can be compressed without losing important constraints.

Consolidation loss should reward semantic memories that predict future useful episodic retrievals.

### Forgetting policy

Forgetting is not deletion alone. It includes decay, scope narrowing, and contradiction marking.

Forgetting triggers:

- low retrieval utility,
- repeated retrieval without contribution,
- contradiction by higher-confidence memory,
- expiry of time-sensitive facts,
- over-specific memory that fails to transfer.

## Training plan

### Stage 1: memory-augmented continued pretraining

Train on long documents and multi-document corpora with segment-level memory.

Losses:

1. next-token prediction,
2. future-span reconstruction from memory,
3. contrastive memory retrieval loss,
4. write/no-write classification loss,
5. memory compression reconstruction loss,
6. redundancy penalty for duplicate memory writes.

The important trick: use memory dropout. Sometimes hide direct context and force the model to recover needed facts from memory. Sometimes hide memory and force local reasoning. This prevents collapse into always-read or never-read behavior.

### Stage 2: synthetic long-range memory curriculum

Generate tasks where the model must remember facts introduced far earlier.

Examples:

- variable binding over 32k-256k tokens,
- entity attribute updates with distractors,
- delayed question answering,
- repeated facts with one later correction,
- multi-hop links spread across segments,
- rule induction from repeated examples.

Labels are deterministic, so memory accuracy and contamination can be measured cleanly.

### Stage 3: supervised memory operation tuning

Train the controller with oracle traces:

- which spans should be written,
- which memory slots should be read,
- when a memory should be consolidated,
- when stale information should be forgotten.

Oracle traces can be generated from known support spans in synthetic tasks and annotated long-context benchmarks.

### Stage 4: instruction tuning with memory constraints

Instruction-tune the model on QA, summarization, code, and reasoning tasks where memory use is budgeted.

The model should learn to answer with the smallest sufficient memory set.

## Evaluation setup

### Baselines

1. Same-size decoder-only Transformer with ordinary attention.
2. Same-size long-context Transformer using full context.
3. Transformer with sliding-window attention only.
4. Mamba or Transformer-Mamba hybrid baseline.
5. Static RAG: retrieve top-k chunks before generation.
6. RETRO-style retrieval-augmented LM, if feasible.
7. Memorizing Transformer-style kNN memory baseline.
8. Recurrent memory-token Transformer baseline.
9. MLA-style attention baseline without persistent memory.
10. Quantized static vector memory baseline using TurboQuant-style compression but no learned write/consolidation gates.
11. Oracle memory retrieval upper bound.
12. MNT without semantic consolidation.
13. MNT without episodic write gate.
14. MNT with always-read memory, to test whether gating matters.

### Benchmarks

Long-context and retrieval:

- LongBench,
- RULER,
- Lost-in-the-Middle tests,
- Needle-in-a-haystack variants,
- NarrativeQA or Qasper,
- HotpotQA / 2WikiMultiHopQA with distractors.

Memory-specific synthetic tasks:

- delayed entity-attribute recall,
- repeated fact consolidation,
- contradiction update tasks,
- sparse relevant memory among many distractors,
- long-range variable binding,
- project-convention recall across sessions.

Language-modeling tasks:

- PG-19 or long-book perplexity,
- long-document continuation,
- code repository continuation with recurring APIs.

### Metrics

Quality:

- exact match / F1 / benchmark score,
- perplexity on long documents,
- multi-hop answer accuracy,
- contradiction handling accuracy.

Memory behavior:

- memory read precision,
- memory read recall,
- write precision,
- write usefulness,
- consolidation precision,
- stale-memory error rate,
- memory contamination rate,
- duplicate memory rate.

Cost:

- input tokens consumed,
- retrieved memory slots,
- KV cache size,
- latent KV size after MLA-style compression,
- memory-slot bits per vector,
- quantization distortion / inner-product error,
- FLOPs proxy,
- latency,
- memory table size.

The best headline metric is quality per memory-token or quality per FLOP, not raw accuracy alone. For the TurboQuant/MLA variant, also report quality per GB of KV-plus-memory state.

## Key ablations

1. No episodic memory: only local context and semantic slots.
2. No semantic memory: only episodic slots.
3. No write gate: write every segment summary.
4. No forget gate: memory only grows.
5. No consolidation loss.
6. No memory dropout during training.
7. Always read top-k memory.
8. Random memory read with same budget.
9. Oracle memory read.
10. Replace learned memory with static RAG text chunks.
11. Replace compact value vectors with raw text snippets.
12. Use the same memory table but inject retrieved memories as text instead of hidden vectors.
13. Remove MLA-style latent KV compression.
14. Replace MLA-style compression with ordinary GQA/MQA.
15. Remove TurboQuant-style memory compression and store full-precision memory slots.
16. Keep TurboQuant-style compression but remove distortion-aware read scoring.
17. Force all memory-attention heads to share one query head, testing whether specialized heads matter.

These ablations separate three questions:

- Does compact memory help?
- Does learned read/write help?
- Does consolidation help beyond episodic retrieval?

## Why this could beat existing LLM architectures

MNT could outperform existing architectures because it addresses a weakness that longer context alone does not fix: raw availability of evidence is not the same as usable memory.

Long-context Transformers still perform a large soft search over raw tokens. They often degrade when relevant evidence is buried in the middle. Static RAG reduces the context size but retrieves before the model fully knows what it needs. KV-cache compression saves memory but does not decide what should become durable knowledge. Agent memory systems often live outside the model and are not optimized by the language-modeling objective.

MNT changes the unit of memory from raw text span to learned memory record. That gives four possible advantages:

1. Better precision: read compact records instead of entire chunks.
2. Better persistence: useful facts survive beyond the immediate context window.
3. Better abstraction: repeated episodes consolidate into semantic memory.
4. Better efficiency: the model consults memory only when the gate predicts value.

Adding TurboQuant-style memory compression and DeepSeek-style MLA makes the idea more viable, not more speculative, because both target the cost bottleneck. MLA suggests a proven route for compressing attention state without discarding multi-head behavior. TurboQuant suggests a plausible route for storing large numbers of memory vectors while controlling similarity distortion. Together, they attack the two costs that would otherwise kill MNT: KV-cache growth and persistent-memory growth.

If this works, it should not merely match a long-context model. It should show a better quality/cost curve on tasks where the relevant evidence is sparse, repeated, or far away.

## Novelty boundary

This is not claiming that memory-augmented neural networks are new. The local corpus already includes related lines:

- Neural Turing Machines and Differentiable Neural Computers: differentiable external memory.
- RAG and RETRO: retrieval as non-parametric memory.
- Memorizing Transformers: kNN memory for Transformers.
- MemGPT: harness-level memory management.
- Mamba/RWKV: efficient long-sequence alternatives.
- DeepSeek-V2/V3: MLA for efficient latent attention and reduced KV-cache cost.
- vLLM/PagedAttention and KV-cache work: inference memory management.
- LongBench and Lost in the Middle: evaluation pressure for long-context memory.
- TurboQuant: online vector quantization with low distortion, relevant for compact memory-slot storage and retrieval.

Quick arXiv checks on 2026-06-16 also surfaced related work on memory-as-a-tool, precision-aware memory retrieval, episodic memory for RAG, KV-cache eviction/compression, latent/reflective memory systems, hardware analysis of DeepSeek MLA, MLA adaptation for Transformer LLMs, and TurboQuant comparison notes.

The narrower claim is this:

MNT combines MLA-style working-memory compression, TurboQuant-style persistent vector compression, learned in-model read/write gates, compact episodic slots, semantic consolidation, forgetting, and memory-specific training losses into one LLM architecture, then evaluates whether that memory hierarchy beats long-context attention and static retrieval under equal budgets.

## Viability assessment

The idea is viable as a research prototype, but only if it is tested in stages. It is not yet viable as a claim that it will beat frontier architectures without evidence.

Why it is viable:

1. The main cost risk has a credible answer.
   - MLA reduces KV-cache pressure for working memory.
   - TurboQuant-style vector quantization can reduce persistent memory-slot cost.
   - Together they make persistent memory less obviously doomed by bandwidth and storage overhead.

2. The architecture has measurable intermediate wins.
   - MLA can be tested before persistent memory.
   - Quantized memory retrieval can be tested before learned write gates.
   - Write/consolidation gates can be tested on synthetic deterministic tasks before real benchmarks.

3. The failure modes are falsifiable.
   - If quantization hurts retrieval too much, full-precision memory should outperform compressed memory.
   - If MLA compression loses long-range detail, full KV or sliding-window baselines should expose it.
   - If memory gates are useless, always-read or oracle-read ablations will show that.

What makes it risky:

1. Training complexity is real. Jointly learning compression, retrieval, writing, consolidation, and language modeling can collapse into always-read, never-write, or summary-spam behavior.
2. Hidden-vector memory is harder to inspect than text retrieval, so debugging requires special probes.
3. Quantization error may damage exactly the subtle similarity signals needed for multi-hop memory retrieval.
4. Semantic consolidation may blur exceptions and create false generalizations.
5. The system may only outperform on synthetic tasks unless the training curriculum matches real long-context use.

Best first test:

Build only the smallest viable variant:

- local/sliding attention plus MLA-style compressed older KV,
- external episodic vector slots,
- TurboQuant-style compressed slot storage,
- learned read gate,
- no semantic consolidation at first,
- no persistent cross-session memory at first.

Success criterion for viability:

MNT-lite must beat static RAG and a sliding-window/MLA baseline on sparse long-range recall and multi-hop synthetic tasks at lower KV-plus-memory bytes for the same answer accuracy. If it cannot win there, the full architecture is not worth building yet.

## Minimal prototype

Do not begin with full-scale pretraining. Build a small controlled prototype.

### Prototype A: external slot memory with hidden-vector injection

- Base model: small open LLM, 0.5B-1.5B.
- Freeze most weights.
- Add LoRA adapters for memory injection.
- Add an external memory table of key/value vectors.
- Add a small memory controller trained on synthetic tasks.
- Inject retrieved memory vectors through gated cross-attention.

Goal: prove learned compact memory beats static text retrieval on synthetic long-range tasks.

### Prototype B: segment-level memory writes

- Split long documents into segments.
- After each segment, write candidate memory slots.
- Train write/no-write labels from future usefulness.
- Evaluate whether the model stores sparse useful facts rather than summaries of everything.

Goal: prove write gating matters.

### Prototype C: consolidation

- Cluster repeated episodic memories.
- Learn semantic slots from repeated useful memories.
- Evaluate tasks where a pattern repeats across episodes but exact surface text changes.

Goal: prove semantic consolidation improves transfer and reduces memory size.

## Practical roadmap

### Week 1: formalize the memory API

Define:

- memory slot schema,
- read scoring function,
- write candidate format,
- consolidation rule,
- forgetting rule,
- metrics for read/write precision.

### Week 2: build synthetic benchmark

Create deterministic tasks for:

- delayed recall,
- contradiction update,
- sparse relevant memory,
- repeated fact consolidation,
- multi-hop memory chains.

### Weeks 3-4: implement prototype A

- Small LLM plus LoRA memory-injection adapter.
- External vector slot table.
- Learned read gate.
- Compare to static RAG and full-context baseline.

### Weeks 5-6: add write gate

- Train write/no-write from future utility.
- Measure duplicate writes, contamination, and recall.

### Weeks 7-8: add semantic consolidation

- Merge recurring episodic memories into semantic slots.
- Test transfer to paraphrased or changed-surface-form tasks.

### Weeks 9-10: run real benchmarks

- LongBench/RULER/Lost-in-the-Middle.
- Multi-hop QA with distractors.
- Long-document perplexity.

### Decision gate

Continue only if MNT beats static RAG or long-context baselines on at least one memory-heavy task family at equal or lower cost.

## Clean research question

Can LLM memory be made architectural rather than contextual: a learned hierarchy of working, episodic, and semantic memory with read/write/consolidate/forget operations that improves long-range reasoning and repeated knowledge use without simply increasing context length?
