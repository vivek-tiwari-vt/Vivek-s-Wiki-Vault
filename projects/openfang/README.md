# OpenFang

OpenFang is an open-source Agent Operating System maintained under https://github.com/RightNow-AI/openfang.

## What it is

- A Rust-based AI platform described as an "Agent Operating System" rather than a chatbot wrapper.
- Publicly positioned as a single-asset, long-running automation system with multiple autonomous components.
- Officially pre-1.0 as of April 2026 (`v0.5.10`) and now at `v0.6.4` (May 2026 release).

## Source-derived facts

- Repo: RightNow-AI/openfang (public).
- Language: Rust (primary).
- Stars/Forks: 17.3k stars, 2.2k forks (GitHub API as of 2026-05-08).
- Workspace metadata license: Apache-2.0 OR MIT.
- Version in workspace metadata: 0.6.4.
- Workspace members: 14 entries in Cargo.toml (`crates/openfang-*` entries plus `xtask`).
- Homepage/docs: https://www.openfang.sh/ with docs and changelog/release notes linked from repo.
- README claims: one binary (~32 MB), 14 crates, autonomous hands, 40 channels, and a dashboard workflow.

## Research additions

- Official docs describe a broader scope: 14 crates, 40 channels, 60 bundled skills, 20 providers, 76 API endpoints.
- Architecture docs describe the workspace subsystem flow across CLI, API, kernel, runtime, skills, and channels.
- Security docs list 16 defense layers (including signed manifests and audit-like chain mechanisms).
- Release note `v0.6.4` adds provider-routing fixes and installer/docs improvements; reports 2,543 passing tests.

## Notes on uncertainty / reconciliation

- README mentions 1,767+ tests; latest release note reports 2,543+ tests.
  This is likely a moving target from later updates.
- Benchmark and comparison tables in README are largely project-authored and should be externally validated.

## Why this matters

OpenFang is relevant as a mature, Rust-native candidate in the autonomous agent platform space if you need:

- autonomous scheduled agents
- integrated channel connectors (Telegram, Discord, Slack, etc.)
- workflow orchestration plus skill/runtime controls

## Cross-links

- [OpenFang setup guide](./setup.md)
- [OpenFang use cases](./use-cases.md)
- [Research notes](./research.md)
- [Alternatives and comparisons](./alternatives.md)
- [Source references](./sources.md)
- [Open questions](./open-questions.md)
- [Agent Operating System concept](../concepts/agent-operating-system.md)
- [RightNow-AI](../companies/rightnow-ai.md)

## Last updated

2026-05-08
