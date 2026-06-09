# Wiki Schema

## Core Rule

One submitted source link becomes one markdown note.

Successful ingests should create or update:

- one primary note in the best-fit folder
- one `index.md` update
- one `log.md` append

Do not create a separate source note plus extra project/company/concept files
for the same link.

## Active Folders

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
- `sources/` only when nothing else fits
- `inbox/failed/` and `inbox/needs-review/` for extraction problems

## Required Frontmatter

Every successful note should include:

- `tags`
- `source_link`
- `context_link`
- `source_type`
- `kind`
- `created`
- `updated`
- `canonical_name`
- `research_sources`
- `last_verified_at`
- `unmapped_terms`

## Grounded Tagging Rule

Do not invent free-form tags.

Tags are categories only, never names of projects, companies, people, repos, or creators.

Allowed families:

- `type/*`
- `domain/*`
- `topic/*`
- `workflow/*`
- `status/*`

Approved type tags:

- `type/project`
- `type/recipe`
- `type/tool`
- `type/company`
- `type/person`
- `type/concept`
- `type/workflow`
- `type/source`
- `type/note`
- `type/reference`

Approved domain tags:

- `domain/ai`
- `domain/automation`
- `domain/knowledge-management`
- `domain/software-engineering`
- `domain/data-engineering`
- `domain/research`
- `domain/productivity`
- `domain/design`
- `domain/infrastructure`
- `domain/security`

Approved topic tags:

- `topic/agent`
- `topic/mcp`
- `topic/llm`
- `topic/prompting`
- `topic/memory`
- `topic/search`
- `topic/rag`
- `topic/knowledge-graph`
- `topic/obsidian`
- `topic/slack`
- `topic/github`
- `topic/api`
- `topic/cli`
- `topic/plugin`
- `topic/integration`
- `topic/auth`
- `topic/storage`
- `topic/vector-search`
- `topic/workspace`
- `topic/automation-bot`

Approved workflow tags:

- `workflow/capture`
- `workflow/ingestion`
- `workflow/summarization`
- `workflow/research`
- `workflow/retrieval`
- `workflow/organization`
- `workflow/monitoring`
- `workflow/automation`
- `workflow/debugging`
- `workflow/documentation`

Approved status tags:

- `status/inbox`
- `status/active`
- `status/reference`
- `status/draft`
- `status/needs-review`
- `status/blocked`
- `status/archived`

### Tag Limits

Every non-recipe note may use:

- exactly `1` `type/*` tag
- up to `3` `domain/*` tags
- up to `3` `topic/*` tags
- up to `2` `workflow/*` tags
- up to `2` `status/*` tags

### Tag Prohibitions

Do not use tags like:

- `project/openfang`
- `company/rightnow-ai`
- `source/githubsignals`
- `tool/system-cleaner`
- `domain/ai-agents`

Those belong in note titles, fields, aliases, or links, not tags.

## Section Requirements

Every non-recipe note must include:

- `## Summary`
- `## What It Is`
- `## What It Does`
- `## How It Works`
- `## Use Cases`
- `## Why It Matters`
- `## Related Tools or Alternatives`
- `## Sources`

Project notes should usually also include:

- `## Source Context`
- `## Notes`

Recipe notes remain the only exception and may use recipe-specific sections.

## Research Requirements

Every non-recipe note must be researched beyond the triggering post or link.

Minimum standard:

- use the source link only as the starting point
- verify the canonical project through official docs, official website, official repo, or other primary sources
- do not stop after copying README taglines
- write an elaborated summary in plain language
- include grounded use cases
- include markdown source links

If evidence is weak or conflicting:

- use `status/needs-review`
- record the uncertainty in the note
- do not fabricate certainty

## Recipe Boundary

Recipe-specific logic belongs only in `recipes/`.

Do not mention:

- ingredients
- servings
- portions
- steps
- cooking details

inside non-recipe notes unless the source is actually a recipe.
