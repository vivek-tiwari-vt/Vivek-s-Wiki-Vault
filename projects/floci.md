---
tags:
  - type/project
  - domain/infrastructure
  - domain/software-engineering
  - topic/integration
  - topic/api
  - topic/github
  - workflow/documentation
  - workflow/ingestion
  - status/reference
source_link: https://github.com/floci-io/floci
context_link: https://github.com/floci-io/floci
source_type: github
kind: project
created: "2026-05-12 09:45:27"
updated: "2026-05-12 09:45:33"
canonical_name: Floci
official_link: https://floci.io
company: floci-io
research_sources:
  - https://github.com/floci-io/floci
  - https://raw.githubusercontent.com/floci-io/floci/main/README.md
  - https://floci.io/floci/getting-started/migrate-from-localstack/
  - https://floci.io/floci/configuration/application-yml/
  - https://floci.io/floci/configuration/storage/
  - https://floci.io/floci/configuration/multi-account/
  - https://floci.io/floci/services/
  - https://api.github.com/repos/floci-io/floci
status: reference
last_verified_at: "2026-05-12"
unmapped_terms: []
---

# Floci

## Summary

Floci is a free, open-source local AWS emulator on Docker, positioned as a practical replacement for LocalStack Community. It aims for broad AWS API coverage, drop-in compatibility, and lower startup/resource costs for local development and testing.

## What It Is

Floci is an actively maintained GitHub project (`floci-io/floci`) with a native image on Docker Hub (`floci/floci`) and MIT-licensed Java codebase. Its public positioning is a local AWS emulation platform with no account requirement and no feature gates.

## What It Does

- Emulates many AWS services locally and supports SDK/tooling compatibility through standard local AWS endpoints (default `http://localhost:4566`).
- Acts as a LocalStack-compatible alternative with faster startup and smaller footprint claims compared to LocalStack Community.
- Supports migration by allowing users to swap image names and endpoint configs rather than refactor application code.
- Provides mixed execution modes:
  - In-process for many control/data APIs.
  - Real Docker-backed containers for services needing protocol fidelity (for example Lambda/ElastiCache/RDS/MSK/EC2/ECS/EKS/etc.).
- Exposes configurable storage behavior for state durability: `memory`, `persistent`, `hybrid`, and `wal`.
- Supports multi-account isolation by using the access key ID as account ID when it is a 12-digit value.

## How It Works

Floci runs as a service on port 4566, routing AWS-style API calls to a router layer and delegated service handlers. Lightweight services can stay in-process, while heavier or protocol-sensitive services are mapped to real containerized backends to preserve behavior (e.g., runtime-specific execution and native protocol handling).

A key design pattern in the project is drop-in compatibility:
- keep SDK/CLI usage unchanged,
- keep ports and endpoint style consistent,
- keep credentials usable in local test/dev patterns,
- add compatibility without requiring a cloud account for emulator access.

## Use Cases

- Local integration testing for S3/SQS/SNS/SSM, ECS/Lambda, RDS/ElastiCache, and similar AWS-heavy stacks.
- Developer onboarding in local-first environments where token-gated alternatives are undesirable.
- CI pipelines needing predictable local emulation for AWS APIs.
- Testing of migration paths from LocalStack Community-style setups.

## Why It Matters

Floci is relevant if your stack depends heavily on AWS APIs but needs deterministic local runs without external dependencies or license gates. Its architecture and compatibility claims make it useful for teams standardizing integration tests that depend on many AWS interactions.

## Related Tools or Alternatives

- LocalStack Community (now documented as requiring token-based restrictions and with frozen security updates per its community migration notice).
- AWS SAM / LocalStack-like emulators for narrower scenarios.
- Other local AWS tools with narrower service scope.

## Sources

- [Floci GitHub repository](https://github.com/floci-io/floci)
- [Floci README (raw)](https://raw.githubusercontent.com/floci-io/floci/main/README.md)
- [Floci homepage](https://floci.io)
- [Migration guide: LocalStack -> Floci](https://floci.io/floci/getting-started/migrate-from-localstack/)
- [Service catalog (docs)](https://floci.io/floci/services/)
- [GitHub API metadata](https://api.github.com/repos/floci-io/floci)

## Source Context

- Trigger source: `https://github.com/floci-io/floci`
- Canonical context was confirmed against the same repo and official project docs.

## Notes

- Repo metadata (stars, language, dates, topics, license) confirmed via GitHub API.
- README declares extensive service coverage and migration behavior; for operational decisions, re-verify version-specific claims against the latest release notes and tags.
- `readme` claims include: 46 AWS services supported, benchmark-style comparisons, and service-specific Docker/runtime details.
- Uncertainty: implementation-level parity depth varies by service; check release changelogs before production-like dependency decisions.