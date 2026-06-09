---
tags:
  - type/project
  - domain/security
  - domain/software-engineering
  - domain/ai
  - topic/cli
  - topic/github
  - topic/llm
  - workflow/debugging
  - workflow/documentation
  - status/reference
source_link: https://www.instagram.com/reel/DXOO5aLjKsv/?igsh=[REDACTED]
context_link: https://github.com/duriantaco/skylos
official_link: https://github.com/duriantaco/skylos
source_type: instagram_reel
kind: project
created: "2026-05-08 15:27:11"
updated: "2026-05-08 15:45:00"
canonical_name: Skylos
company: duriantaco
status: reference
research_sources:
  - https://github.com/duriantaco/skylos
  - https://docs.skylos.dev
  - https://pypi.org/project/skylos/
last_verified_at: "2026-05-08"
unmapped_terms: []
---

# Skylos

## Summary

Skylos is a local-first static analysis and CI-gating project aimed at modern codebases that are increasingly edited or generated with AI assistance. Its pitch is broader than a simple linter: dead code detection, secrets scanning, security checks, quality gates, AI-generated-code mistake detection, and LLM-application defenses all live behind one command surface. That makes Skylos relevant not only as a scanner, but as a model for how AI-era code review tooling is expanding beyond traditional style or vulnerability checks.

## What It Is

Skylos is an open-source static analysis and PR-gating tool that can run locally, in CI, through GitHub Actions, and through editor or MCP integrations. It is built primarily in Python and supports scanning across several major programming languages.

## What It Does

- Finds dead code such as unused functions, classes, imports, files, and route-level mistakes.
- Flags security issues including common injection or traversal classes, plus secret exposure.
- Performs quality checks around complexity, brittle patterns, and regression-style code smells.
- Adds AI-focused checks for hallucinated helpers, missing controls, prompt-injection exposure, and incomplete safety patterns.
- Generates CI workflows, supports diff-aware scans, and exposes optional AI-assisted review features.

## How It Works

Skylos combines several scanner categories behind one CLI. The repo and docs describe dead-code analysis, security analysis, rule references, CI/CD generation, technical debt reporting, AI-assisted review, and an AI-defense surface for LLM applications. The project also emphasizes framework awareness and diff-aware gating, which is important because AI-generated regressions often show up in changed lines or in framework-specific entrypoints rather than in isolated files.

## Use Cases

- Adding a local-first quality and security gate to repositories where AI tools generate or heavily modify code.
- Detecting dead code and stale branches after large AI-assisted refactors.
- Enforcing pull-request checks in GitHub Actions without piecing together many separate tools.
- Extending static-analysis workflows to cover LLM application risks such as unsafe tool use or missing output validation.

## Why It Matters

Skylos matters because AI-assisted development changes the error distribution of code review. Teams still need classic static analysis, but they also need checks for hallucinated APIs, removed validation, missing auth, or hidden debt introduced by rapid machine-generated edits. Skylos is trying to unify those surfaces in one workflow instead of treating AI-generated-code problems as totally separate from the rest of secure development.

## Related Tools or Alternatives

- Semgrep, CodeQL, Bandit, and Vulture for narrower slices of static analysis.
- GitHub Advanced Security and similar code-scanning platforms for enterprise CI integration.
- Editor-native linters and language servers when the goal is faster feedback but not a broader CI policy surface.

## Sources

- [Skylos GitHub repository](https://github.com/duriantaco/skylos)
- [Skylos documentation](https://docs.skylos.dev)
- [Skylos PyPI package](https://pypi.org/project/skylos/)

## Source Context

- Trigger source: Instagram reel from `githubsignals`
- Source framing: security and quality scanner for AI-generated code
- Canonical research source: official repo and documentation

## Notes

- The project has a notably broad surface area, so teams evaluating it should separate what is available in the core local-first scanner from what depends on optional extras or integrations.
- This note was rewritten to reflect the full documented product surface rather than only the social-post framing.
