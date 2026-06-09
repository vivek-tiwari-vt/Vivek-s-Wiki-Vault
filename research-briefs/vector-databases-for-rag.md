---
tags: [type/reference, domain/ai, domain/data-engineering, topic/rag, topic/vector-search, topic/storage, workflow/retrieval, status/reference]
source_link: "https://raunaqness.substack.com/p/vector-databases-for-rag"
context_link: "https://raunaqness.substack.com/p/vector-databases-for-rag"
source_type: "substack-article"
kind: "research-brief"
created: 2026-06-09
updated: 2026-06-09
canonical_name: "Vector Databases for RAG"
research_sources:
  - "https://raunaqness.substack.com/p/vector-databases-for-rag"
  - "https://github.com/pgvector/pgvector"
  - "https://docs.trychroma.com/docs/overview/introduction"
  - "https://qdrant.tech/documentation/search/filtering/"
  - "https://docs.pinecone.io/guides/search/filter-by-metadata"
  - "https://lancedb.github.io/lancedb/"
last_verified_at: 2026-06-09
unmapped_terms: []
---

# Vector Databases for RAG

## Summary

Raunaq’s article explains why vector database choice for RAG is mostly a filtering and storage-architecture decision, not merely a nearest-neighbor-search benchmark. The central issue is filtered vector search: production RAG usually asks for “nearest chunks within this user, tenant, source, or date range,” while vector indexes and metadata indexes are often separate structures.

The practical takeaway is that simple systems such as pgvector or Chroma are excellent for prototypes and modest workloads, but selective metadata filtering, multi-tenancy, and datasets larger than memory quickly expose architectural differences. Qdrant and Pinecone are framed as stronger fits for production filtered search; LanceDB is framed as compelling for disk-native ML/data workflows with versioning and Arrow-style columnar access.

## What It Is

A comparative technical brief on vector databases for RAG, covering pgvector, ChromaDB, Qdrant, Pinecone, and LanceDB. It focuses on how each system stores vectors and metadata, how it handles filtering, and where each architecture fits.

## What It Does

- Explains the “filtering problem” in vector search: vector similarity and metadata predicates often need to be satisfied together.
- Compares post-filtering, pre-filtering, and filtered index traversal.
- Shows how pgvector, ChromaDB, Qdrant, Pinecone, and LanceDB make different trade-offs.
- Provides selection guidance by workload: prototype, multi-tenant SaaS, managed production, SQL-centric app, or large disk-native ML pipeline.

## How It Works

The article separates filtered vector retrieval into three broad strategies:

1. Post-filtering: run the vector index first, then remove candidates that fail metadata filters. This is simple but can silently under-deliver when filters are selective.
2. Pre-filtering: apply metadata filters first, then brute-force vector search over the matching subset. This is correct but can become slow when the subset is large.
3. Filtered index traversal: combine metadata eligibility with vector-index traversal, often using bitmaps or payload indexes, and adaptively fall back to brute force when appropriate.

System notes from the article:

- pgvector keeps vectors inside Postgres and uses HNSW/IVFFlat indexes alongside normal SQL metadata indexes. This gives SQL simplicity but can struggle when the planner must choose between vector and metadata indexes.
- Chroma stores metadata in SQLite and vectors in hnswlib. Filtered queries pre-filter through SQLite and brute-force over matching IDs, giving correctness at local/prototype scale but limited filtered-query performance at scale.
- Qdrant stores data in segments with vector storage, payload storage, HNSW indexes, and payload indexes. It uses payload-index-derived eligibility during search and can adapt between filtered HNSW traversal and brute force.
- Pinecone emphasizes managed operation, namespaces, and metadata filtering for multi-tenant SaaS-style workloads.
- LanceDB uses Lance columnar storage, colocating vectors and metadata in Arrow-oriented files with versioning, making it attractive for local/serverless/offline ML workflows and datasets large relative to RAM.

## Use Cases

- Choosing a vector database for a RAG system with user-level or tenant-level access controls.
- Deciding when pgvector is sufficient versus when a purpose-built vector database is warranted.
- Designing retrieval architecture where metadata selectivity and recall matter.
- Evaluating whether embedded/local vector storage or managed vector infrastructure is the better operational fit.

## Why It Matters

RAG correctness depends on retrieving the right eligible chunks, not just any semantically similar chunks. A retrieval layer that silently returns too few filtered results can make the LLM appear weak when the real failure is retrieval architecture. The article is useful because it turns “which vector DB should I use?” into a more precise set of workload questions: filter selectivity, tenancy model, operational preference, RAM constraints, and reproducibility needs.

## Related Tools or Alternatives

- [[CocoIndex]] for keeping source-derived context fresh before it reaches vector storage.
- [[Agentmemory]] and [[Memoir]] for agent memory systems where retrieval semantics, tenancy, and persistence matter.
- pgvector, ChromaDB, Qdrant, Pinecone, and LanceDB as the main compared retrieval backends.

## Source Context

- Source author: Raunaq.
- Source date visible in extracted article: 2026-06-02.
- Extraction status: public article extracted successfully; source text and headings were available.
- Research enrichment: official project/docs pages for pgvector, Chroma, Qdrant filtering, Pinecone metadata filtering, and LanceDB were checked for canonical context.

## Notes

Selection heuristic preserved from the source:

- Prototype or one-user internal tool: pgvector or Chroma.
- Large filtered subsets: Qdrant.
- Multi-tenant product: Qdrant if self-hosting, Pinecone if managed service is preferred.
- Managed scale with less operational burden: Pinecone.
- SQL joins and loose filters: pgvector.
- Large disk-native datasets, versioning, Arrow interoperability, or offline ML evaluation: LanceDB.

## Sources

- [Vector Databases for RAG - Raunaq’s Substack](https://raunaqness.substack.com/p/vector-databases-for-rag)
- [pgvector GitHub repository](https://github.com/pgvector/pgvector)
- [Chroma documentation](https://docs.trychroma.com/docs/overview/introduction)
- [Qdrant filtering documentation](https://qdrant.tech/documentation/search/filtering/)
- [Pinecone metadata filtering documentation](https://docs.pinecone.io/guides/search/filter-by-metadata)
- [LanceDB documentation](https://lancedb.github.io/lancedb/)
