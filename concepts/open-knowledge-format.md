---
tags:
  - type/concept
  - domain/ai
  - domain/knowledge-management
  - topic/agent
  - topic/obsidian
  - topic/rag
  - workflow/ingestion
  - status/reference
source_link: https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing
context_link: https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf
source_type: multi-source
kind: concept
created: "2026-06-14"
updated: "2026-06-14"
canonical_name: Open Knowledge Format
research_sources:
  - https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing
  - https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
  - https://gist.githubusercontent.com/karpathy/442a6bf555914893e9891c11519de94f/raw
  - https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf
  - https://raw.githubusercontent.com/GoogleCloudPlatform/knowledge-catalog/main/okf/README.md
  - https://raw.githubusercontent.com/GoogleCloudPlatform/knowledge-catalog/main/okf/SPEC.md
last_verified_at: "2026-06-14"
unmapped_terms: []
---

# Open Knowledge Format

## Summary

Open Knowledge Format (OKF) is Google Cloud’s draft v0.1 formalization of the LLM-wiki pattern: a portable directory of markdown files with YAML frontmatter, ordinary markdown links, optional `index.md` files, optional `log.md` files, and a minimal conformance surface so humans, agents, catalogs, and graph/search tools can exchange curated operational knowledge without a proprietary service or SDK.

The three links supplied together are one canonical topic:

- Google Cloud blog post: introduces OKF and explicitly says it formalizes Karpathy’s LLM-wiki pattern.
- Andrej Karpathy gist: describes the original LLM Wiki pattern — a persistent, compounding markdown knowledge base maintained by an LLM.
- GoogleCloudPlatform `knowledge-catalog/okf`: publishes the OKF specification, proof-of-concept enrichment agent, visualizer, and sample bundles.

## What It Is

OKF is a file format and bundle convention for representing knowledge around data, systems, APIs, runbooks, metrics, datasets, and other concepts. It is intentionally simple: a knowledge bundle is just a directory tree of UTF-8 markdown files. Each concept document has YAML frontmatter at the top and markdown body content underneath.

The OKF v0.1 spec describes the unit of distribution as a **Knowledge Bundle** and each markdown file as a **Concept**. Concept IDs are derived from bundle-relative file paths without the `.md` suffix. For example, `tables/users.md` has concept ID `tables/users`.

The spec’s required frontmatter is deliberately minimal:

```yaml
---
type: <Type name>
title: <Optional display name>
description: <Optional one-line summary>
resource: <Optional canonical URI for the underlying asset>
tags: [<tag>, <tag>]
timestamp: <ISO 8601 datetime>
---
```

Only `type` is required. Recommended fields are `title`, `description`, `resource`, `tags`, and `timestamp`. Producers can add extra keys, and consumers are expected to tolerate unknown types and unknown frontmatter fields gracefully.

## What It Does

OKF gives agent builders and data teams a common interchange format for knowledge that is currently scattered across metadata catalogs, wikis, shared drives, code comments, notebooks, runbooks, and senior engineers’ heads.

Its practical functions are:

- Make knowledge readable by humans without special tooling.
- Make knowledge parseable by agents without bespoke SDKs.
- Make knowledge diffable and reviewable in git.
- Preserve provenance and history through markdown links, citations, `log.md`, and version control.
- Allow progressive disclosure through directory-level `index.md` files so an agent does not need to load an entire corpus at once.
- Express relationships as a graph using standard markdown links, not only parent/child folder hierarchy.
- Separate producers from consumers: one tool can emit OKF, another can render it, another can index it, and another can reason over it.

## How It Works

An OKF bundle is a directory tree:

```text
bundle/
├── index.md
├── log.md
├── <concept>.md
└── <subdirectory>/
    ├── index.md
    ├── <concept>.md
    └── ...
```

Reserved filenames:

- `index.md`: optional directory listing for progressive disclosure.
- `log.md`: optional chronological update history.

All other markdown files are concept documents.

Concept documents carry structured metadata in YAML frontmatter and use the markdown body for details, schemas, examples, citations, joins, runbooks, and prose. Links can be absolute bundle-relative links like `/tables/customers.md` or normal relative links like `./other.md`. OKF recommends absolute bundle-relative links when stability matters.

The Google Cloud reference implementation under `GoogleCloudPlatform/knowledge-catalog/okf` includes:

- An enrichment agent package named `enrichment-agent`.
- Google ADK and Gemini-based proof-of-concept implementation.
- BigQuery source support as the first source implementation.
- Two-pass enrichment: a BigQuery metadata pass followed by an LLM/web pass over explicit seed URLs.
- A static HTML visualizer that turns OKF bundles into interactive graph views.
- Sample bundles for GA4 ecommerce, Stack Overflow public data, and Bitcoin public datasets.

## Use Cases

- Data catalogs: represent datasets, tables, metrics, schemas, joins, and examples in a portable form.
- Agent context packs: give agents curated markdown knowledge that can be loaded, traversed, and updated as files.
- Metadata-as-code: store knowledge near the code and data assets it describes, reviewed through PRs.
- Cross-tool interchange: move curated knowledge between Obsidian, GitHub, MkDocs, search indexes, graph viewers, and agent runtimes.
- Runbooks and playbooks: represent operational processes as concept documents linked to systems, datasets, and alerts.
- Knowledge ingestion pipelines: output normalized markdown bundles instead of locking enriched context inside one vendor catalog.

## Why It Matters

Karpathy’s LLM Wiki pattern argues that plain RAG repeatedly rediscovers knowledge at query time, while a maintained wiki compiles knowledge once and keeps it current. The LLM reads raw sources, updates summaries and entity/concept pages, adds cross-links, notes contradictions, and files useful answers back into the wiki.

OKF turns that pattern into a shared interchange format. The key shift is from **one bespoke wiki maintained by one agent** to **bundles that many tools can produce and consume**. Google Cloud’s blog frames the missing piece as “a format, not another service”: anyone should be able to produce, consume, exchange, version, and inspect knowledge as files.

For this vault, OKF is directly relevant because the current Obsidian/wiki pattern already follows similar conventions: markdown pages, frontmatter, an index, a log, source links, duplicate handling, and agent-maintained synthesis. OKF provides a possible external compatibility target for exporting or standardizing those notes later.

## Related Tools or Alternatives

- Karpathy’s LLM Wiki pattern: agent-maintained markdown wiki over raw sources.
- Obsidian vaults and other markdown-first knowledge bases.
- Metadata catalogs such as Dataplex, Unity Catalog, Collibra, and internal data catalogs.
- RAG systems that index raw documents but do not maintain compiled summaries.
- Static-site documentation systems such as MkDocs, Hugo, and Jekyll.
- Knowledge graphs and graph viewers layered over markdown links and frontmatter.

## Source Relationship

These three submitted sources are not separate topics:

1. **Karpathy gist** — origin/pattern layer: explains why an LLM-maintained wiki is useful and how it differs from RAG.
2. **Google Cloud blog** — announcement/positioning layer: says OKF formalizes the LLM-wiki pattern into a portable, interoperable, vendor-neutral format.
3. **GoogleCloudPlatform repository** — implementation/spec layer: contains the OKF v0.1 spec, README, proof-of-concept enrichment agent, sample bundles, and visualizer.

Canonical handling: this note treats OKF as the canonical concept and keeps Karpathy’s gist as the conceptual predecessor/source lineage rather than a separate note.

## Notes

- OKF v0.1 is explicitly a draft and starting point, not a finished standard.
- OKF is a format, not a platform: no central authority, schema registry, required runtime, proprietary account, or required SDK.
- The reference enrichment agent is only one producer example; the OKF README stresses that the format itself is the contribution.
- The `schema/okf-schema.json` path was checked during ingest and returned 404; the authoritative spec is `okf/SPEC.md`, not a JSON schema file at that path.
- Google Cloud says Knowledge Catalog has been updated to ingest OKF and serve it to agents, but this ingest did not test the product integration.
- License for the repository is Apache-2.0 according to GitHub and `okf/LICENSE.md`.

## Sources

- Google Cloud announcement: https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing
- Karpathy LLM Wiki gist: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
- Karpathy raw gist: https://gist.githubusercontent.com/karpathy/442a6bf555914893e9891c11519de94f/raw
- OKF repo folder: https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf
- OKF README: https://raw.githubusercontent.com/GoogleCloudPlatform/knowledge-catalog/main/okf/README.md
- OKF SPEC.md: https://raw.githubusercontent.com/GoogleCloudPlatform/knowledge-catalog/main/okf/SPEC.md
