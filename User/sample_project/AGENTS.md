# AGENTS.md

Project-scope instruction file for Codex agents working in this project root.

This project root may contain multiple repositories, services, modules, experiments, or tooling folders. These preferences are specific to the user's workflow and should be followed when they apply to the requested task.

These instructions are project-level defaults. If a child repository contains its own `AGENTS.md`, read and follow that repository-specific file as the more local source of truth for work inside that repository.

## 1. Python Environment

For local Python execution, use the existing conda environment named `base_AI`.

Prefer running Python commands through the environment explicitly:

```bash
conda run -n base_AI python ...
conda run -n base_AI pytest ...
```

Do not create a new conda environment, virtual environment, or dependency setup unless explicitly requested.

When installing, checking, or running packages locally, assume `base_AI` is the target Python environment unless the user says otherwise.

If a specific repository has its own documented environment, dependency file, Docker setup, or local `AGENTS.md`, follow the repository-specific instruction when working inside that repository.

## 2. Docker Workflow

When adding or modifying Docker-based processing, use Docker Compose.

Prefer implementing or updating a `docker-compose.yml` file instead of relying on standalone `docker run` commands.

Containers should be designed to:
- Build successfully with Docker Compose.
- Stay running after startup.
- Avoid automatically launching training, inference, preprocessing, or batch-processing jobs.
- Allow the user to enter the container manually with `docker compose exec ... bash`.
- Let the user manually launch processing commands from inside the container.

Do not process, build, run, restart, stop, remove, or prune Docker containers, images, volumes, or networks automatically unless explicitly requested.

Do not configure expensive or long-running processing jobs to start automatically on `docker compose up` unless explicitly requested.

If a process must be launched automatically, explain why this is necessary before implementing it.

## 3. Path Handling

For new or modified Python code, use `pathlib.Path` for filesystem paths.

Prefer:

```python
from pathlib import Path

data_dir = Path("data")
metadata_path = data_dir / "metadata.csv"
```

Avoid adding new path logic based on string concatenation, such as:

```python
metadata_path = "data/" + filename
```

When editing existing code, convert path handling to `Path` only where it is directly related to the requested change.

Do not refactor unrelated path code only for style consistency.

For project-level code that references child repositories or modules, prefer paths relative to the project root when that is the existing convention. Do not hard-code machine-specific absolute paths unless explicitly requested.

## 4. Project Review Document

This folder is a global project root that may contain multiple repositories or modules.

Maintain a global project review document at:

```text
docs/project_review.md
```

This document is a living summary of the full project. It should help future coding agents quickly understand the project root, included repositories, shared workflows, and cross-repository relationships before making changes.

### 4.1 Startup Behavior

At the beginning of each task, check whether the following path exists:

```text
docs/project_review.md
```

If `docs/project_review.md` exists:
- Read it before making code changes.
- Use it to understand the project objective, project-level structure, included repositories/modules, shared conventions, and known constraints.
- Do not assume it is perfectly up to date; verify against the actual project files when needed.

If `docs/project_review.md` does not exist:
- Create the `docs/` directory if needed.
- Review the project root structure and identify important child repositories, modules, scripts, services, configuration files, and documentation.
- Generate `docs/project_review.md` before or alongside the requested change.
- Keep the first version practical and concise rather than exhaustive.

Use this command pattern when creating the folder/file manually:

```bash
mkdir -p docs
touch docs/project_review.md
```

If the requested work targets a child repository that has its own repository-level review file, such as:

```text
<repository>/docs/code_review.md
```

then read that repository-level review file as well. Use the project review for global context and the repository review for local implementation details.

### 4.2 Required Content

The `docs/project_review.md` file should include these sections:

```md
# Project Review

## 1. Project Objective

Summarize the main purpose of the overall project root.

## 2. Project Structure

Describe the important top-level folders, repositories, modules, services, scripts, and documentation files.

## 3. Repository and Module Map

List the major child repositories or modules and summarize the responsibility of each one.

## 4. Cross-Repository Relationships

Explain how the repositories, modules, services, shared data, shared configs, or shared tooling relate to each other.

## 5. Main Execution Flow

Explain the main project-level workflows, including how scripts, modules, services, containers, or pipelines are expected to run together.

## 6. Key Components

Summarize the major classes, functions, modules, pipelines, services, or entry points that are important across the project.

## 7. Data, Inputs, and Outputs

Describe important input files, output files, datasets, metadata files, model files, generated artifacts, and shared storage assumptions.

## 8. Environment and Execution

Document relevant Python, conda, Docker, Docker Compose, GPU, NAS, database, cloud, external-service, and OS assumptions.

## 9. Current Implementation Notes

Record important implementation details that future agents should know before editing this project.

## 10. Recent Changes

Summarize meaningful recent changes made by coding agents.

## 11. Known Issues, Risks, and TODOs

List known bugs, fragile areas, incomplete logic, risky assumptions, migration needs, or future cleanup items.
```

### 4.3 Update Behavior After Each Task

After implementing each user request, update `docs/project_review.md` so it reflects the latest project state.

Update only the sections affected by the change.

Examples:
- If a new repository or module is added, update `Project Structure`, `Repository and Module Map`, or `Cross-Repository Relationships`.
- If a new script, service, or entry point is added, update `Main Execution Flow` or `Key Components`.
- If Docker behavior changes, update `Environment and Execution`.
- If data paths, metadata formats, model locations, input files, or output files change, update `Data, Inputs, and Outputs`.
- If a known issue is discovered but not fixed, update `Known Issues, Risks, and TODOs`.
- If the change is meaningful, add a short note to `Recent Changes`.

Do not rewrite the whole file unless the existing document is clearly outdated or structurally incorrect.

Do not add noisy entries for trivial formatting-only changes.

If the change affects only a child repository and that repository has its own `docs/code_review.md`, update the repository-level review file as well as the project-level review file when the project-level understanding changes.

### 4.4 Style Rules

Keep `docs/project_review.md`:
- Accurate.
- Concise.
- Project-specific.
- Useful for future agents.
- Focused on current behavior, not speculation.

Prefer clear summaries over long explanations.

When uncertain, inspect the actual files before updating the document.

Do not describe implementation details that were not verified from the project files unless clearly marked as assumptions.

### 4.5 Scope Control

Maintaining `docs/project_review.md` should not cause broad unrelated refactoring.

The project review document is documentation, not a reason to modify code.

If updating the document would require a large project investigation unrelated to the user's request, make the smallest useful update and mention any remaining uncertainty.

## 5. Multi-Repository Work Rules

Treat each child repository or module as a separate local context unless the user explicitly asks for cross-repository changes.

Before editing files:
- Identify which repository, module, or folder the requested task belongs to.
- Check whether that repository has its own `AGENTS.md`.
- Follow the most local applicable instructions.
- Avoid modifying sibling repositories unless the task requires it.

When a task spans multiple repositories:
- State the affected repositories or modules.
- Keep changes separated by repository when possible.
- Preserve existing public interfaces between repositories unless the user explicitly asks to change them.
- Update shared documentation when cross-repository behavior changes.

Do not move files across repositories or reorganize module boundaries unless explicitly requested.

## 6. Instruction Precedence

Use the following precedence order when instructions conflict:

1. The user's explicit instruction for the current task.
2. The nearest `AGENTS.md` file in the target repository or module.
3. This project-scope `AGENTS.md`.
4. Existing code style and documented project conventions.
5. General best practices.

If following a project-level rule would create unnecessary complexity for a small local change, prefer the smaller correct change and mention the tradeoff.

## 7. Safety and Change Control

Prefer small, reviewable changes.

Do not perform destructive actions unless explicitly requested, including:
- Deleting repositories, modules, branches, large folders, datasets, model files, or generated artifacts.
- Running cleanup commands that remove Docker volumes, caches, build outputs, or untracked files.
- Rewriting Git history.
- Changing credentials, secrets, access tokens, or deployment settings.

Do not edit files containing secrets or credentials unless the user explicitly asks and the change is necessary.

If secrets are found accidentally, do not print them. Mention only that sensitive values appear to exist and identify the file path if needed.

## 8. Verification

For code changes, use the smallest relevant verification step available.

Prefer repository-local verification when the task affects a single repository.

Examples:
- Run a focused unit test instead of the full test suite when sufficient.
- Run a syntax check for the changed Python files when tests are not available.
- Validate Docker Compose files with a non-destructive config check when Docker changes are made, if the user has allowed Docker commands.
- Inspect affected paths and imports after moving or renaming files.

Do not run expensive training, inference, batch processing, full dataset jobs, Docker builds, or long integration tests unless explicitly requested.

If verification cannot be run safely or is outside the allowed scope, state what should be checked and why it was not run.

## 9. Final Response Expectations

When finishing a task, summarize:
- What changed.
- Which repositories, modules, or files were affected.
- What verification was performed.
- Any follow-up risks, assumptions, or manual steps.

Keep the final response concise and focused on the user's request.
