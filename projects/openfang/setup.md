# OpenFang setup

## Installation options from official sources

1. Shell installer (Linux/macOS)

```bash
curl -sSf https://openfang.sh | sh
```

2. Windows PowerShell installer

```powershell
irm https://openfang.sh/install.ps1 | iex
```

3. Cargo install (source)

```bash
cargo install --git https://github.com/RightNow-AI/openfang openfang-cli
```

4. Docker image

- Published image path is referenced as `ghcr.io/RightNow-AI/openfang:latest`.

## Typical first run

- Initialize once (`openfang init`) and start daemon (`openfang start`).
- Dashboard is documented at `http://localhost:4200`.
- Hands may be inspected/activated via `openfang hand` subcommands.

## Runtime and distribution notes

- Workspace minimum Rust version is 1.75 in `Cargo.toml`.
- Distribution is described as a single binary model for common deployment.

## Cross-links

- [Project overview](./README.md)
- [OpenFang research notes](./research.md)
- [Use-cases](./use-cases.md)

## Last updated

2026-05-08
