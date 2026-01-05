# AGENTS.md

This repository is a **Gemini CLI extension** (“Conductor”). Most “code” here is **prompt/protocol text** stored in TOML files and markdown templates.

## What this repo contains

- **Extension manifest**: `gemini-extension.json`
  - `contextFileName` points to `GEMINI.md` (the default context the CLI loads).
- **Command prompts**: `commands/conductor/*.toml`
  - Each file defines a `description` and a large `prompt = """ ... """` body that acts as an operational protocol for an AI agent.
- **Templates**: `templates/`
  - `templates/workflow.md` is the default workflow Conductor copies into user projects.
  - `templates/code_styleguides/` contains language-specific styleguide templates.
- **Release packaging**: `.github/workflows/package-and-upload-assets.yml`
  - Publishes `conductor-release.tar.gz` containing the repo contents (excluding `.git` and `.github`).

## Editing guidelines (read this before changing files)

### Command prompt files (`commands/conductor/*.toml`)

- **Treat prompts as user-facing protocol**: keep instructions precise, sequential, and unambiguous.
- **Preserve TOML structure**:
  - Keep `description = "..."` and `prompt = """ ... """`.
  - Do not introduce unescaped `"""` inside prompt bodies.
- **Avoid behavior that can hang tools**: prompts should prefer one-shot, non-watch commands and avoid “run dev server forever” instructions.
- **Avoid repo-specific assumptions**: these prompts execute inside *a user’s* project, not this extension repo. Don’t hardcode paths or tools unless they’re part of the Conductor contract.
- **Be conservative with “CRITICAL” directives**: add them only when necessary, and make sure they don’t contradict each other.

### Templates (`templates/**`)

- Keep templates **generic** and **copy/paste friendly** (they’re copied into user projects).
- If you change `templates/workflow.md`, ensure it still reads like a template:
  - It may include examples, but should clearly label them as examples.
  - Avoid requiring specific languages/tools unless the template section says to adapt it.

### Versioning and packaging

- If changes materially affect behavior, consider bumping the extension version in `gemini-extension.json`.
- The release workflow archives almost the entire repo; avoid adding large or sensitive files.

## Local validation (recommended)

There is no test suite in this repo, but you can still validate that files are well-formed:

- **Parse TOML command files** (syntax check):

```bash
python - <<'PY'
import pathlib, tomllib
for p in pathlib.Path("commands/conductor").glob("*.toml"):
    tomllib.loads(p.read_text(encoding="utf-8"))
print("OK: TOML parsed")
PY
```

- **Validate JSON manifest**:

```bash
python -m json.tool gemini-extension.json >/dev/null && echo "OK: JSON parsed"
```

- **Packaging sanity check** (matches the GitHub workflow):

```bash
tar -czf /tmp/conductor-release.tar.gz --exclude='.git' --exclude='.github' .
tar -tzf /tmp/conductor-release.tar.gz | head
```

## Common pitfalls

- **Accidentally breaking TOML quoting** in prompt bodies (most common).
- **Editing prompts in ways that create contradictions** (e.g., “don’t run any tool calls” vs “run tool calls now”).
- **Adding repository-specific instructions** to prompts that are intended to run in arbitrary user projects.

