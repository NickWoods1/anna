# 2026-07-06 Python Architecture Preferences

## Objective

- Update `developing-in-python` with Nick's Python structure preferences.

## Current State

- The skill now prefers frozen Pydantic configs for new Python development.
- The skill now describes a dependency hierarchy separating config/constants, pure state transforms/functions, orchestration/wiring, and entry points.

## Key Decisions

- Put the preferences directly in `SKILL.md` because they are core behavior, not optional reference material.
- Keep the guidance flexible enough to respect existing projects while making the preferred shape explicit for new work.

## Files Changed

- `skills/developing-in-python/SKILL.md`
- `sessions/2026-07-06-python-architecture-preferences.md`

## Verification

- Ran `python3 /home/nick/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/developing-in-python`.
- Validation passed.

## Next Steps

- Use the skill on a real Python project and refine the structure guidance if it proves too strict or too vague.
