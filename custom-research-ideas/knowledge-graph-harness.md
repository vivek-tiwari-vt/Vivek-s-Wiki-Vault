# Knowledge Graph Harness: Claude-style Tool-first Design (Initial Concept)

Idea summary:

Create a dedicated knowledge-graph harness that ingests a folder of files, takes a prompt describing what extraction focus to apply, builds a scalable knowledge graph, provides visualizations, and offers a chat/query interface.

Core model:
- 96% tooling and programmatic behavior.
- 4% AI, constrained to interpretation and enrichment.

Agentic flow to mirror Claude Code patterns:
- Gather context from files and metadata.
- Extract entities/relations deterministically where possible.
- Validate and normalize graph writes.
- Use AI only for ambiguous cases and relationship enrichment.
- Persist everything with checkpoints so operations are recoverable.

What the harness should provide:
1. Folder ingest pipeline
   - Recursive file discovery with filters (extensions, ignore rules, path include/exclude).
   - Deterministic preprocess/parsing per file type.
   - File-level fingerprints and incremental reprocessing.

2. Graph extraction engine
   - Ontology-driven entity and relationship schema.
   - Deterministic rules plus constrained AI pass where rules are insufficient.
   - Provenance on every node and edge (source path, span, extractor, confidence).

3. Query/chat interface
   - Natural-language query entry that resolves to structured graph queries.
   - Hybrid retrieval: explicit graph traversal + vector semantic search fallback.
   - Ranked output with cited evidence.

4. Visualization layer
   - Obsidian-like graph visualization (interactive nodes/edges, filters, clustering).
   - View by project, relation type, confidence, and freshness.
   - Export to Obsidian-compatible markdown links or PNG/SVG for reporting.

5. Feature-extensibility surface (how you instruct harness behavior)
   - Always-loaded baseline config (project conventions, safety rules).
   - On-demand skills/plugins for domain-specific extraction focus.
   - Hooks for pre/post-ingest, validation, and approvals.
   - New features declaratively added through prompt/skill updates.

6. Scalability design for millions of docs and entities
- Sharded storage layer (graph DB + vector DB + metadata store).
- Background workers with backpressure, retries, checkpoints, and resumable runs.
- Batching, queue-based scheduling, and immutable versioned graph snapshots.
- Caching for parser outputs and embeddings.

Safety and governance patterns (Claude-style lessons):
- Permission modes (read-only, ask-first, auto-accept for trusted commands).
- Undo checkpoints before structural mutations.
- Session resume/fork plus deterministic replay.
- Audit trail and rollback of graph transactions.

Result: a production-grade, graph-first AI agent framework where AI is mostly an orchestrator/assessor, while deterministic tool operations do most of the work.

This is the same philosophy used by modern agentic coding tools:
- strong tool loop
- controlled tool execution
- session/state management
- extensible command and plugin model
- explicit safety gates
