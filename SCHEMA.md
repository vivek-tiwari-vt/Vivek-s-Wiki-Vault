# Wiki Schema

## Domain
Set the domain explicitly before large ingests. Until then, this wiki uses the
source-wiki-builder default ingestion schema on top of llm-wiki.

## Orientation
- Read `SCHEMA.md`, `index.md`, and recent `log.md` before creating pages.
- Search for existing slugs and aliases before creating a canonical page.
- Prefer canonical pages over duplicate notes.

## Core Files
- `index.md` is the top-level catalog.
- `log.md` is append-only.
- `sources/` stores source notes.
- `inbox/` stores pending, failed, and needs-review notes.

## Canonical Folders
- `projects/`
- `recipes/`
- `tools/`
- `products/`
- `companies/`
- `people/`
- `concepts/`
- `workflows/`
- `tutorials/`
- `comparisons/`
- `research-briefs/`

## Naming
- Lowercase file names
- Hyphenated slugs
- Normal markdown links for cross-links

## Update Rules
- Create source notes for every ingested source.
- Update `index.md` for every new canonical page.
- Append to `log.md` for every ingest or major update.
- Do not delete old pages unless explicitly requested.
- Commit after every successful ingest.
