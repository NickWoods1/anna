# 2026-07-06 Python Toolchain

## Objective

- Update `developing-in-python` to require `uv`, `ruff`, and `pyrefly` as the preferred Python toolchain.
- Install those tools locally so Codex can use them.

## Current State

- The skill now names `uv` for package management, environments, locks, and tool execution.
- The skill now names `ruff` for linting and formatting.
- The skill now names `pyrefly` for type checking.

## Key Decisions

- Prefer `uv run` for project commands.
- Prefer `uv add`, `uv add --dev`, `uv sync`, and `uv lock` for dependency work.
- Preserve existing non-uv projects unless the user asks to migrate or the task requires it.

## Files Changed

- `skills/developing-in-python/SKILL.md`
- `sessions/2026-07-06-python-toolchain.md`

## Verification

- Ran `python3 /home/nick/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/developing-in-python`.
- Validation passed.
- Installed and verified `uv 0.11.26`.
- Installed and verified `ruff 0.15.20`.
- Installed and verified `pyrefly 1.1.1`.

## Next Steps

- Use the toolchain on the next Python project and refine the skill if command conventions need tightening.
