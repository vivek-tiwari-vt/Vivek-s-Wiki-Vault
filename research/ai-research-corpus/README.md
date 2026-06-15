---
title: AI Research Corpus
type: research-corpus
status: reference
created: 2026-06-15
source: local-curation
---

# AI Research Corpus

This folder stores the modern AI research corpus moved out of the WattsNext prototype project so the project `data/` folder stays clean.

## Contents

- `curated_modern_ai_research_papers.md` — lab-segregated curated list with paper links, blog links, category tags, and one-line impact notes.
- `raw/papers/` — downloaded paper PDFs.
- `raw/source-link-cards/` — text cards for blog links, GitHub/model-card links, product pages, and non-PDF paper sources.
- `ingestion-artifacts/` — manifest and download log from the download run.

## Move verification

Moved from:

- `/Users/vivektiwari-nexus3/prototype/data/input/ai_research_papers`
- `/Users/vivektiwari-nexus3/prototype/data/input/ai_research_links`
- `/Users/vivektiwari-nexus3/prototype/data/artifacts/ai_research_ingestion`
- `/Users/vivektiwari-nexus3/prototype/curated_modern_ai_research_papers.md`

Current expected counts:

- PDFs: 142 files
- Source-link cards: 137 files
- Ingestion artifacts: 2 files

## Notes

No pre-existing prototype `data/input`, `data/artifacts`, or `data/markdown` folders were deleted. Only the AI-research subfolders created during the prior download run were moved.
