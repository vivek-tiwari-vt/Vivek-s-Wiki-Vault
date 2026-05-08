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

## Tag Conventions

Prefer nested tags for graph view and filtering:

- `type/project`
- `type/recipe`
- `source/github`
- `project/openfang`
- `company/rightnow-ai`
- `status/pre-1-0`

## Default Sections

Most notes should include:

- Summary
- What It Is
- What It Does
- What It's Used For
- Key Details
- Notes

Recipe notes should include:

- Summary
- Recipe Details
- Ingredients
- Steps
- Notes
