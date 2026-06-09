---
tags: [type/project, domain/ai, domain/data-engineering, topic/rag, topic/vector-search, topic/storage, workflow/retrieval, status/reference]
source_link: "https://github.com/RyanCodrai/turbovec"
context_link: "https://github.com/RyanCodrai/turbovec"
source_type: "github-repository"
kind: "project"
created: 2026-06-09
updated: 2026-06-09
canonical_name: "TurboVec"
research_sources:
  - "https://github.com/RyanCodrai/turbovec"
  - "https://raw.githubusercontent.com/RyanCodrai/turbovec/main/README.md"
  - "https://raw.githubusercontent.com/RyanCodrai/turbovec/main/docs/api.md"
  - "https://raw.githubusercontent.com/RyanCodrai/turbovec/main/docs/integrations/langchain.md"
  - "https://pypi.org/project/turbovec/"
  - "https://crates.io/crates/turbovec"
  - "https://arxiv.org/abs/2504.19874"
last_verified_at: 2026-06-09
unmapped_terms: []
---

# TurboVec

## Summary

TurboVec is a Rust vector index with Python bindings, built around Google Research’s TurboQuant quantization approach. Its purpose is to make local vector search for RAG materially smaller in memory while preserving practical recall and SIMD-accelerated search speed.

The project’s central claim is straightforward: a large float32 embedding corpus can be compressed into a much smaller 2-bit or 4-bit representation, searched locally, and filtered at search time without a managed vector database. This makes TurboVec especially relevant to privacy-sensitive, memory-constrained, air-gapped, or low-latency RAG deployments.

## What It Is

TurboVec is an open-source vector-search project hosted at `RyanCodrai/turbovec`. The repository describes it as a vector index built on TurboQuant, implemented in Rust and exposed through Python bindings.

The repository provides:

- a Rust crate named `turbovec`;
- a Python package named `turbovec`;
- a positional `TurboQuantIndex`;
- an `IdMapIndex` for stable external `u64` ids;
- serialization formats for `.tv` and `.tvim` indexes;
- integrations for LangChain, LlamaIndex, Haystack, and Agno;
- benchmark material comparing recall, compression, and speed against FAISS-style product quantization baselines.

## What It Does

- Compresses vectors using TurboQuant-style quantization at 2-bit or 4-bit widths.
- Supports online ingest: vectors can be added without a separate training step or full rebuild.
- Performs local nearest-neighbor search with SIMD kernels targeting ARM NEON and x86 AVX-512BW paths.
- Allows filtered search by passing an id allowlist or mask into search, so retrieval can be restricted to candidates from SQL, BM25, ACLs, tenant filters, or time windows.
- Persists indexes to disk and reloads them for later use.
- Provides Python and Rust APIs for application integration.

## How It Works

TurboVec’s README explains the TurboQuant process in four broad steps:

1. Normalize vectors and store the norm separately, turning each vector into a unit direction.
2. Apply a shared random orthogonal rotation so coordinates follow a predictable distribution in high dimensions.
3. Use per-coordinate calibration, described by the project as TQ+, to adjust finite-dimensional coordinate drift.
4. Apply Lloyd-Max scalar quantization with 2-bit or 4-bit buckets.

At query time, the compressed representation is searched using SIMD kernels. For filtered retrieval, TurboVec can accept a mask or allowlist. The project states that filtering happens inside the SIMD kernel at block granularity, avoiding work for blocks with no allowed slots and dropping non-allowed slots before result insertion.

The API exposes two main index types:

- `TurboQuantIndex`: a compact positional index where vectors are addressed by insertion slot. Deletes use `swap_remove`, so external slot references can change.
- `IdMapIndex`: a stable-id wrapper that maps external `u64` ids to the underlying index and supports O(1) removal by id.

## Use Cases

- Local RAG retrieval where data should remain on the machine or inside a private VPC.
- Memory-constrained embedding search where storing every vector as float32 is too expensive.
- Air-gapped RAG stacks paired with open-source embedding models.
- Hybrid retrieval pipelines where SQL, BM25, ACL, or metadata filters produce a candidate id set and TurboVec performs dense reranking over that set.
- Framework-native prototyping through LangChain, LlamaIndex, Haystack, or Agno integrations.
- Comparing local quantized search against heavier vector database deployments in privacy- or latency-sensitive systems.

## Why It Matters

TurboVec sits directly at the intersection of [[Vector Databases for RAG]] and local agent memory infrastructure. The prior vector-database note emphasizes that filtered retrieval is often the real production challenge. TurboVec’s design is interesting because it is not a full metadata database; instead, it is a compact local dense reranker that can consume candidate ids from another system and apply filtering during search.

That makes it a useful architectural component when the metadata source of truth remains elsewhere—Postgres, SQLite, BM25, ACL tables, or a document store—but dense similarity search needs to be small, fast, and local.

## Related Tools or Alternatives

- [[Vector Databases for RAG]] for broader vector database trade-offs across pgvector, ChromaDB, Qdrant, Pinecone, and LanceDB.
- FAISS, especially product quantization and `IndexPQ` / FastScan-style baselines.
- Qdrant or Pinecone when the system needs a complete vector database with metadata indexing and service-oriented operation.
- LanceDB when the design needs disk-native columnar storage and dataset versioning.
- pgvector when SQL integration and operational simplicity outweigh specialized quantized local search.

## Source Context

- Source repository: `RyanCodrai/turbovec`.
- Repository description from GitHub API: “A vector index built on TurboQuant, written in Rust with Python bindings.”
- Repository status at verification: public, MIT-licensed, default branch `main`, created 2026-03-26, pushed 2026-06-09.
- GitHub API reported primary language as Python, with language bytes split between Python and Rust.
- GitHub topics included `ann`, `embedding`, `embeddings`, `faiss`, `nearest-neighbor`, `quantization`, `rag`, `rust`, `simd`, `turboquant`, and `vector-search`.
- Package context checked through PyPI and crates.io. PyPI reported `turbovec` version `0.7.0`; crates.io reported max Rust crate version `0.8.0` at verification time.
- The linked TurboQuant paper was verified through arXiv as “TurboQuant: Online Vector Quantization with Near-optimal Distortion Rate.”

## Notes

Implementation details worth preserving:

- Python install: `pip install turbovec`.
- Rust install: `cargo add turbovec`.
- Use `IdMapIndex` rather than `TurboQuantIndex` when application ids must survive deletes.
- `TurboQuantIndex.swap_remove()` is O(1) but changes slot positions, which can invalidate external slot references.
- In the Python API reference, `TurboQuantIndex.search(..., mask=...)` restricts results by slot mask; `IdMapIndex.search(..., allowlist=...)` restricts results by external ids.
- LangChain integration exposes `turbovec.langchain.TurboQuantVectorStore` as a replacement for an in-memory vector store, with metadata filters resolved to an id allowlist before scoring.

Uncertainties and cautions:

- Benchmark claims are project-provided and should be independently tested on the target hardware and embedding distribution before production adoption.
- The project is young, with active recent pushes and no GitHub releases returned by the releases API at verification time, though tags were present.
- TurboVec is best understood as a local vector index/reranker, not a complete vector database with durable metadata indexing, distributed service operation, or built-in access-control semantics.

## Sources

- [TurboVec GitHub repository](https://github.com/RyanCodrai/turbovec)
- [TurboVec README](https://raw.githubusercontent.com/RyanCodrai/turbovec/main/README.md)
- [TurboVec API reference](https://raw.githubusercontent.com/RyanCodrai/turbovec/main/docs/api.md)
- [TurboVec LangChain integration](https://raw.githubusercontent.com/RyanCodrai/turbovec/main/docs/integrations/langchain.md)
- [TurboVec on PyPI](https://pypi.org/project/turbovec/)
- [TurboVec on crates.io](https://crates.io/crates/turbovec)
- [TurboQuant paper on arXiv](https://arxiv.org/abs/2504.19874)
