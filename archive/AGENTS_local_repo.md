# AGENTS.md

Project scope instruction file for Codex agents working in this repository.

These preferences are specific to this user's workflow and should be followed when they apply to the requested task.

## 1. Python Environment

For local Python execution, use the existing conda environment named `base_AI`.

Prefer running Python commands through the environment explicitly:

```bash
conda run -n base_AI python ...
conda run -n base_AI pytest ...
```

Do not create a new conda environment, virtual environment, or dependency setup unless explicitly requested.

When installing or checking packages, assume `base_AI` is the target local Python environment unless the user says otherwise.

## 2. Docker Workflow

When adding or modifying Docker-based processing, use Docker Compose.

Prefer implementing or updating a `docker-compose.yml` file instead of relying on standalone `docker run` commands.

Containers should be designed to:

- Build successfully with Docker Compose.
- Stay running after startup.
- Avoid automatically launching training, inference, preprocessing, or batch-processing jobs.
- Allow the user to enter the container manually with `docker compose exec ... bash`.
- Let the user manually launch processing commands from inside the container.

Do not process, build, run any docker commands automatically. 

## 3. Path Handling

For new or modified Python code, use `pathlib.Path` for filesystem paths.

Prefer:

```python
from pathlib import Path

data_dir = Path("data")
metadata_path = data_dir / "metadata.csv"
```

Do not refactor unrelated path code only for style consistency.

## 4. Repository Review Document

Maintain a repository review document at:

```text
docs/code_review.md
```

This document is a living summary of the repository. It should help future coding agents quickly understand the project before making changes.

### 4.1 Startup Behavior

At the beginning of each task, check whether the following path exists:

```text
docs/code_review.md
```

If `docs/code_review.md` exists:
- Read it before making code changes.
- Use it to understand the project objective, structure, major modules, conventions, and known constraints.
- Do not assume it is perfectly up to date; verify against the actual repository when needed.

If `docs/code_review.md` does not exist:
- Create the `docs/` directory if needed.
- Review the repository structure and key files.
- Generate `docs/code_review.md` before or alongside the requested change.
- Keep the first version practical and concise rather than exhaustive.

Use this command pattern when creating the folder/file manually:

```bash
mkdir -p docs
touch docs/code_review.md
```

### 4.2 Required Content

The `docs/code_review.md` file should include these sections:

```md
# Code Review

## 1. Project Objective

Summarize the main purpose of the repository.

## 2. Repository Structure

Describe the important folders and files.

## 3. Main Execution Flow

Explain how the main scripts, modules, services, or containers work together.

## 4. Key Components

Summarize the major classes, functions, modules, pipelines, or services.

## 5. Data, Inputs, and Outputs

Describe important input files, output files, datasets, metadata files, model files, or generated artifacts.

## 6. Environment and Execution

Document relevant Python, conda, Docker, Docker Compose, GPU, NAS, database, or external-service assumptions.

## 7. Current Implementation Notes

Record important implementation details that future agents should know before editing.

## 8. Recent Changes

Summarize meaningful recent changes made by coding agents.

## 9. Known Issues, Risks, and TODOs

List known bugs, fragile areas, incomplete logic, risky assumptions, or future cleanup items.
```

### 4.3 Update Behavior After Each Task

After implementing each user request, update `docs/code_review.md` so it reflects the latest repository state.

Update only the sections affected by the change.

Examples:
- If a new script is added, update `Repository Structure`, `Main Execution Flow`, or `Key Components`.
- If Docker behavior changes, update `Environment and Execution`.
- If data paths, metadata formats, or outputs change, update `Data, Inputs, and Outputs`.
- If a known issue is discovered but not fixed, update `Known Issues, Risks, and TODOs`.
- If the change is meaningful, add a short note to `Recent Changes`.

Do not rewrite the whole file unless the existing document is clearly outdated or structurally incorrect.

Do not add noisy entries for trivial formatting-only changes.

### 4.4 Style Rules

Keep `docs/code_review.md`:
- Accurate.
- Concise.
- Repository-specific.
- Useful for future agents.
- Focused on current behavior, not speculation.

Prefer clear summaries over long explanations.

When uncertain, inspect the actual code before updating the document.

### 4.5 Scope Control

Maintaining `docs/code_review.md` should not cause broad unrelated refactoring.

The repository review document is documentation, not a reason to modify code.

If updating the document would require a large repository investigation unrelated to the user's request, make the smallest useful update and mention any remaining uncertainty.
