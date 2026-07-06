---
name: developing-in-python
description: Pragmatic Python development workflow for Codex using uv for package management, ruff for linting/formatting, and pyrefly for type checking. Use when working on Python projects, scripts, packages, tests, CLIs, data workflows, automation, or backend services; when editing .py, pyproject.toml, uv.lock, pytest/unittest suites, ruff config, pyrefly config, or Python packaging; or when the user asks to implement, debug, refactor, test, review, or explain Python code.
---

# Developing in Python

## Core Workflow

1. Discover the project shape before changing code.
   - Check `pyproject.toml`, `uv.lock`, `requirements*.txt`, `setup.py`, `setup.cfg`, `tox.ini`, `noxfile.py`, `Makefile`, and CI config.
   - Identify the test runner, Python version, and any existing exceptions to the default `uv` + `ruff` + `pyrefly` toolchain.
   - Prefer existing commands from docs, Makefile, task runner config, or CI over inventing new ones.

2. Make scoped changes.
   - Match the project's existing architecture, naming, imports, error handling, and typing style.
   - Keep public APIs stable unless the user asked for an API change.
   - Avoid broad dependency, formatting, or lockfile churn unless needed for the task.
   - Use standard-library tools when they are adequate; add dependencies only when they remove meaningful complexity.
   - For new Python development, begin from a frozen Pydantic config in a `core/` package unless the existing project structure clearly dictates otherwise.

3. Verify with the narrowest useful checks first, then broaden when risk warrants it.
   - Run the affected tests before the whole suite when the suite may be slow.
   - Run `ruff` and `pyrefly` checks for Python changes unless the project cannot support them yet; note the blocker if skipped.
   - If a command fails because dependencies are missing, inspect the project's setup path and report the exact blocker.

## Environment Handling

- Use `uv` for package management, dependency resolution, virtual environments, lockfiles, and Python tool execution.
- Prefer `uv run <tool>` for project commands so tools run inside the project environment.
- Prefer `uv add`, `uv add --dev`, `uv remove`, `uv sync`, and `uv lock` over direct `pip`, Poetry, Pipenv, or Conda commands.
- Do not install packages globally when a project environment is available.
- If a virtual environment must be created, use `uv venv` and keep `.venv` untracked.
- Treat `uv.lock` as intentional. Update it only when dependencies changed or `uv` requires it.
- If a repo already uses another package manager, migrate only when the user asks or the task requires it; otherwise preserve behavior and document the mismatch.

## Architecture Preferences

- Prefer a clear dependency hierarchy. Low-level modules should not import orchestration, entry points, UI, CLI, web handlers, or infrastructure code.
- For new projects or substantial new subsystems, start with a `core/` package containing the durable domain model, frozen Pydantic config, constants, and pure behavior.
- Keep constants, settings, and configuration definitions in one obvious place, usually `core/config.py` or a small `core/config/` package.
- Make configs immutable by default. Use frozen Pydantic models for application configuration and pass config objects explicitly instead of reading environment or globals throughout the codebase.
- Keep state transforms, validation, parsing, and pure functions separate from wiring and side effects.
- Keep orchestration, I/O, framework glue, job runners, and entry-point wiring in separate modules above the core layer.
- Make entry points consistent. CLIs, scripts, services, workers, and notebooks should all construct config, initialize dependencies, call the same orchestration layer, and return or exit in predictable ways.
- When adding a new directory or package, make its role clear from the name and keep imports flowing in one direction:

```text
entrypoints -> orchestration/wiring -> core transforms/functions -> core config/constants/models
```

- Do not split code into tiny layers mechanically. Use the hierarchy to keep dependencies understandable, tests direct, and side effects contained.

## Comprehensibility

- Optimize for code that fits in a human working set. A reader should be able to understand one behavior without loading the whole project into memory.
- Prefer small vertical slices that can be run, tested, and reviewed end to end over broad speculative scaffolding.
- Use names, module boundaries, and function signatures to make intent explicit; avoid hidden coupling, ambient globals, and surprising side effects.
- Keep functions short enough to scan, but do not fragment a coherent idea just to reduce line count.
- Make invalid states hard to represent with typed config, explicit domain models, and narrow interfaces.
- Use checklists for repeated change workflows: discover structure, identify boundaries, make the change, test the slice, then broaden verification.
- Treat tests as executable explanations. Tests should show behavior through clear setup, meaningful assertions, and stable examples.
- When code becomes hard to explain in a short hand-off, reduce scope, rename concepts, extract a boundary, or simplify the data flow before adding more behavior.

## Testing and Debugging

- For pytest projects, prefer targeted commands like:

```bash
uv run pytest path/to/test_file.py::test_name
```

- For unittest projects, use:

```bash
uv run python -m unittest path.to.test_module
```

- When fixing a bug, try to reproduce it with a failing test or minimal command before editing.
- Add or update tests when behavior changes, regressions are likely, or the bug is user-visible.
- Keep tests deterministic: avoid real network calls, wall-clock assumptions, current working directory leaks, and order dependence unless the project already handles them.

## Python Code Standards

- Prefer clear, boring Python over clever constructs.
- Use explicit data shapes: frozen Pydantic models for config, and dataclasses, TypedDict, Pydantic models, or simple dictionaries for other data according to existing project style.
- Preserve exception context unless intentionally converting errors for a public API.
- Use context managers for files, temporary directories, locks, subprocesses, and resources that need cleanup.
- Avoid mutable default arguments.
- Avoid import-time side effects in libraries and testable modules.
- Prefer pathlib for filesystem paths when surrounding code uses it; otherwise follow local convention.
- For CLIs, keep parsing, business logic, and side effects separable enough to test.

## Type Checking

- Use `pyrefly` as the default type checker for Python projects.
- Follow the project's current typing strictness.
- Add annotations where they clarify contracts or satisfy existing type checks.
- Do not perform large annotation drives as part of unrelated work.
- Use `typing.Protocol`, generics, or overloads only when they simplify real call sites or public contracts.

## Linting and Formatting

- Use `ruff` as the default linter and formatter.
- Prefer `uv run ruff check .` for linting.
- Prefer `uv run ruff format .` for formatting when formatting is requested or needed.
- Avoid introducing Black, isort, Flake8, or other overlapping format/lint tools in new projects unless the existing repo already depends on them.

## Dependencies

- Before adding a dependency, check whether the repo already has an equivalent package or helper.
- For runtime dependencies, use `uv add <package>`.
- For dev-only tools, use `uv add --dev <package>`.
- After dependency changes, run `uv lock` or `uv sync` as appropriate.

## Hand-Off

When finishing meaningful Python work, summarize:

- What changed and why.
- Which files matter.
- Which verification commands passed or failed.
- Any environment assumptions, missing dependencies, or follow-up risks.
