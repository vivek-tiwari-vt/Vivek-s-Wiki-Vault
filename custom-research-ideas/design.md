# Knowledge Graph Harness (KG Harness) — MVP Design and Build Plan

Current objective:
Build a first runnable MVP of a folder-aware, prompt-driven knowledge-graph harness, prioritized for tool-first behavior and safe recoverability.

Folder scope:
- /Users/vivektiwari-nexus3/Wiki/custom-research-ideas/

Deliverable set (MVP)
1) architecture.md (this document)
2) MVP runbook and CLI contract
3) Minimal ingestion + query + visualization path

Folder-level schema (minimal)
- Input:
  - folder_path: absolute or relative path to scan
  - focus_prompt: short natural-language instructions on extraction scope
  - file filters: include_exts, exclude_globs, min_size_bytes, max_size_bytes, max_depth
  - execution_mode: dry_run | run | validate
  - output_target: graph_project_id
  - dry_run: true/false
- Output:
  - graph_db snapshot
  - extraction report JSON
  - evidence manifest (source file -> spans -> extracted fact)
  - run log with checkpoints

Minimal module map (first runnable)
1) cli/
   - kg-harness.py (entrypoint)
   - commands: init, ingest, query, visualize, session, plugin
2) core/
   - collector.py (files discovery + metadata)
   - parser.py (text extraction by type)
   - extractor.py (deterministic + constrained AI pass)
   - normalizer.py (ID normalization + dedupe)
   - graph_writer.py (persist nodes/edges)
   - session.py (resume/fork/checkpoint tokens)
   - validators.py (schema + safety checks)
3) storage/
   - graph/store_adapter.py (Neo4j/pgvector-ready abstraction)
   - graph/schema.cypher or sqlddl (initial schema)
   - checkpoints/
4) query/
   - planner.py (nl-to-query stub or rules first)
   - query_graph.py (explicit traversal)
   - query_semantic.py (placeholder vector search)
5) ui/
   - visualize.py (Cytoscape/D3 JSON export)
   - export_obsidian.py (markdown note graph bridge)
6) plugins/
   - registry.py (on-demand extractor packs)
   - builtins/: default rules and relation sets
7) hooks/
   - pre_ingest.py
   - post_ingest.py
   - on_conflict.py

Phase 1 MVP spec (8-week target)
- Week 1: bootstrap
  - Initialize project skeleton and typed configuration format
  - CLI command wiring and local config loader
  - Ingestion dry-run and run modes
- Week 2: deterministic collector
  - Recursive scan
  - Ignore handling (.git, node_modules, dist, build, .venv, .obsidian)
  - Parser adapters: markdown, txt, py/js/ts, json, yaml
  - File fingerprints (sha256)
- Week 3: graph schema + writes
  - Node types: Document, Section, Concept, Entity, RelationshipClaim
  - Edge types: MENTIONS, LINKS_TO, DEFINES, RELATES_TO
  - Confidence + provenance fields
  - Idempotent upsert by stable deterministic IDs
- Week 4: extraction policy + rules
  - Rule templates derived from focus_prompt:
    - header/entity extraction
    - link extraction
    - definition-like pattern capture
  - Constraint: only emit JSON from extractor
- Week 5: validation and checkpoints
  - Pre-write schema validation
  - Commit-style run checkpoint snapshots
  - Resume/fork behavior
  - Rollback command
- Week 6: query MVP
  - Deterministic graph query modes
    - /by-node
    - /neighbors
    - /path
  - Hybrid entrypoint stub with semantic search flagged as optional
- Week 7: visualization
  - JSON graph export format for visualization viewer
  - One-page local HTML showing nodes/edges, filtering by relation type and confidence
- Week 8: production-hardening
  - Plugin loading + extension points
  - Performance profile and load test on 1M-line synthetic corpus
  - Docs: quickstart and safety checklist

Data model (MVP)
Node fields:
- id (stable hash)
- kind (document, section, entity, relation_claim)
- canonical_name
- aliases[]
- source_refs[]
- attributes (json)
- created_at, updated_at

Edge fields:
- id
- src_node_id
- dst_node_id
- rel_type
- confidence (0..1)
- source_span (file, start, end, snippet)
- extractor_id
- schema_version
- created_at, updated_at

Run contract
Input file:
- path: "../project-folder"
- focus_prompt: "Extract architecture components, APIs, and risk edges"

Output:
- run_id: uuid
- ingest_count: number of files scanned
- accepted_entities
- rejected_candidates
- graph_delta:
  - nodes_added
  - edges_added
  - edges_merged
- evidence_count
- checkpoint_id
- rollback_command

MVP guardrails (tool-first policy)
- Never write graph changes without:
  1) schema validation
  2) extractor policy rule pass
  3) provenance attachment
- AI pass limited to:
  - relation suggestion only
  - label normalization in fallback
  - no free-form graph edits
- Confidence thresholds:
  - rule-generated: auto-accept
  - AI-generated: auto-accept only above threshold; otherwise queue for review

Extension model (future-safe)
- skills/: declarative prompt packs for domain extraction
- plugins/: optional Python modules with extractor hooks
- hooks/: lifecycle scripts (pre, post, conflict, approve)
- permission profile: trusted command allowlist for ingestion side effects

Scalability path for next phase
- Move from in-process writes to worker queue (RQ/Celery-like)
- Shard by project/workspace
- Add vector sidecar for semantic search
- Add incremental incrementalism using modified time + hash diff
- Add relation indexing + pagerank-style ranking

Success criteria for first runnable MVP
- Command runs:
  - kg-harness ingest --path ./sample --focus "..."
  - kg-harness query "..."
- Completes on multi-folder corpus with deterministic results
- Produces graph with provenance for all accepted edges
- Generates checkpoint before every mutable transaction
- Supports resume from interrupted runs
- Renders at least one interactive visualization

Risks and rollback
- Scope drift:
  - Risk: focus prompt interpreted too loosely
  - Rollback: force JSON schema + review mode for low confidence
- Data noise:
  - Risk: low-quality files create false entities
  - Rollback: file trust tiers and allowlist filters
- Scale pressure:
  - Risk: memory spikes on large corpora
  - Rollback: batch size caps, streaming parser, queueed workers

Immediate next files to create next after this design (recommended)
1) schema.md with formal graph schema definitions
2) cli-spec.md with command contracts and option examples
3) runbook.md with one-command setup + demo corpus test
4) checkpoints.md with rollback and recovery procedures

Status: Draft design complete for first runnable MVP; ready for implementation planning into tasks.