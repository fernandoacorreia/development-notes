# Python Code Quality

## Comprehensive linting

Linting must take into account all file types, and it must catch common AI agent mistakes.

- Check that dependencies are properly pinned in pyproject.toml
- Check that uv.lock is synchronized (uv lock --locked)
- Reject TODO in comments
- Reject large files
- Reject line numbers in comments
- Run custom, project-specific checks
- Run pre-commit hooks
- Run source code auto-formatter (python, yaml, json, typescript, etc.)
- Run static analyzers / type checkers
- Sort *.toml files
- After everything ran: Check for untracked files (CI only)

## Black

The uncompromising Python code formatter.

- https://github.com/psf/black
- https://black.readthedocs.io/en/stable/

## Codecov

Code coverage, unit test generation, flaky test detection.

- https://about.codecov.io/

## deptry

Find unused, missing and transitive dependencies in a Python project.

- https://github.com/fpgmaas/deptry/

## Mypy

Mypy is a static type checker for Python.

- https://mypy.readthedocs.io/en/stable/

See: https://docs.pydantic.dev/2.0/api/mypy/ (Pydantic MyPy plugin)

Hints:
- Use `strict = true`.
- Additional recommended rules: warn_unreachable, warn_no_return, disallow_any_generics, disallow_subclassing_any, disallow_any_unimported, disallow_any_expr, disallow_any_decorated, disallow_any_explicit, extra_checks.

## pre-commit

A framework for managing and maintaining multi-language pre-commit hooks.

- https://pre-commit.com/

## ruff

An extremely fast Python linter and code formatter, written in Rust.

- https://github.com/astral-sh/ruff

Most useful rules: A, ANN, ASYNC, B, C4, E, ERA, F, FA, FLY, FURB, G004, I, ICN, LOG, N, PERF, PIE, PLC0415, PLC*, RET, RSE,
S101, S102, S103, S301, S324, S501, SIM, T20, TID, UP, W.

## toml-sort

Toml sorting library.

- https://github.com/pappasam/toml-sort
- https://toml-sort.readthedocs.io/en/latest/

## ty

An extremely fast Python type checker, written in Rust.

https://docs.astral.sh/ty/

