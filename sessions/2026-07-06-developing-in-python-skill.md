# 2026-07-06 Developing in Python Skill

## Objective

- Create a `developing-in-python` Codex skill for pragmatic Python implementation, debugging, testing, and review work.

## Current State

- Skill exists at `skills/developing-in-python/`.
- The skill includes `SKILL.md` plus `agents/openai.yaml`.
- No extra scripts, references, or assets were added.

## Key Decisions

- Keep the skill instruction-only for now; Python development guidance is mostly workflow and standards rather than deterministic scripts.
- Focus the skill on project discovery, scoped changes, environment handling, testing, type checking, dependency changes, and final hand-offs.
- Use repo-local skill storage rather than installing into `~/.codex/skills`.

## Files Changed

- `skills/developing-in-python/SKILL.md`
- `skills/developing-in-python/agents/openai.yaml`
- `sessions/2026-07-06-developing-in-python-skill.md`

## Verification

- Ran `python3 /home/nick/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/developing-in-python`.
- Validation passed.

## Next Steps

- Use the skill on real Python tasks and refine it if repeated friction appears.
