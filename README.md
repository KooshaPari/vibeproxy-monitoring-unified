# vibeproxy-monitoring-unified

Unified monitoring configuration for VibeProxy services. (See `SPEC.md`.)

## Status

Scaffolding only. No monitoring configurations, dashboards, or alert rules are checked in yet — this repository currently holds governance, tooling configs, and the specification document.

## Layout

| Path | Purpose |
|------|---------|
| `SPEC.md` | Specification: scope and intended contents |
| `AGENTS.md` | Agent governance |
| `CLAUDE.md` | Claude Code project instructions |
| `FUNCTIONAL_REQUIREMENTS.md` | FR tracker (stub) |
| `cliff.toml` | git-cliff changelog config |
| `mise.toml` | mise tool-version config |
| `.pre-commit-config.yaml` | pre-commit hooks |
| `.editorconfig` | Editor config |
| `docs/worklogs/` | Per-session worklog entries |

## Quick Start

```bash
git clone https://github.com/KooshaPari/vibeproxy-monitoring-unified.git
cd vibeproxy-monitoring-unified
mise install     # installs tool versions declared in mise.toml
pre-commit install
```

## Links

- Canonical repo: https://github.com/KooshaPari/vibeproxy-monitoring-unified
