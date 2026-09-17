# AGENTS.md

Behavioral guidelines for coding agents.

### Work Environment & Operational Rules

- The current host OS is Windows 11 and execution environment is Linux (team workstation). The repository is accessed through VS Code Remote - SSH from the Windows host. 
- Do not overstate or understate user requests, reply with objective viewpoint based from trusted sources.
- For repositories configured with uv, use the local repo env to run simple Python commands.
- For repositories configured with Docker, do not execute any docker commands that actually run or build any containers or services. You are allowed to run other docker commands that inspect status, see logs, etc.
- Do not create virtual environment, install packages into a system Python, or replace the given interpreter unless the user explicitly requests it.

### Markdown Document Date Management

- For every Markdown (`.md`) document that you create or modify, maintain document metadata immediately below the top-level title. Use the following format:
```md
# Document Title

**Created:** YYYY-MM-DD  
**Updated:** YYYY-MM-DD

...
```
- When creating a new Markdown document:
  - Set `Created` to the current date.
  - Set `Updated` to the current date.
- When modifying an existing Markdown document:
  - Preserve the existing `Created` date exactly.
  - Set `Updated` to the current date.
- If an existing Markdown document does not contain this metadata:
  - Determine the creation date from reliable repository information if readily available, such as Git history.
  - If a reliable creation date cannot be determined without significant investigation, use the current date for `Created`.
  - Set `Updated` to the current date.
- Place the metadata immediately after the first H1 (`# ...`) heading and before the document body.
- Do not add duplicate metadata blocks.
- Do not change `Updated` when merely reading or inspecting a document.
- Only update the dates when the file itself is actually modified.
- When editing a Markdown file, verify before finishing that the metadata exists and reflects the resulting document state.
- Do not apply this rule to generated, vendored, third-party, dependency, or externally maintained Markdown files unless the task explicitly requires editing their metadata.
