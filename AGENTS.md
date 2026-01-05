# AGENTS.md

This file provides guidance for AI agents working with the Conductor extension codebase.

## Project Overview

**Conductor** is a Gemini CLI extension that enables **Context-Driven Development**. It transforms the Gemini CLI into a proactive project manager that follows a structured protocol: **Context → Spec & Plan → Implement**.

This repository contains:
- Command definitions (TOML files with embedded prompts)
- Templates for code style guides and workflows
- Extension configuration and documentation

## Project Structure

```
/workspace/
├── commands/conductor/       # Command definitions (TOML)
│   ├── setup.toml           # Project scaffolding command
│   ├── newTrack.toml        # Create new feature/bug track
│   ├── implement.toml       # Execute track tasks
│   ├── status.toml          # Show project progress
│   └── revert.toml          # Git-aware revert command
├── templates/
│   ├── code_styleguides/    # Language-specific style guides
│   │   ├── general.md
│   │   ├── python.md
│   │   ├── typescript.md
│   │   ├── javascript.md
│   │   ├── go.md
│   │   ├── dart.md
│   │   └── html-css.md
│   ├── git_hooks/           # Git hook templates
│   └── workflow.md          # Default workflow template
├── gemini-extension.json    # Extension manifest
├── GEMINI.md               # Context file for Gemini CLI
├── README.md               # User documentation
└── CONTRIBUTING.md         # Contribution guidelines
```

## Key Files

### Command Files (`commands/conductor/*.toml`)

Each command file is a TOML file with two main fields:
- `description`: Brief description shown in command help
- `prompt`: The full system prompt that defines the command's behavior

When editing command prompts:
- Maintain the existing markdown formatting with numbered sections
- Keep the `CRITICAL` and `PROTOCOL` markers for important instructions
- Preserve the step-by-step structure that guides the AI agent
- Test changes by running the actual commands via Gemini CLI

### Template Files (`templates/`)

Templates are copied to user projects during `/conductor:setup`:
- `workflow.md`: Defines TDD workflow, commit guidelines, quality gates
- `code_styleguides/*.md`: Language-specific coding conventions

When editing templates:
- These become part of user projects, so changes affect all new setups
- Keep templates self-contained and well-documented
- Include practical examples where helpful

### Extension Manifest (`gemini-extension.json`)

```json
{
  "name": "conductor",
  "version": "0.1.1",
  "contextFileName": "GEMINI.md"
}
```

- `name`: Extension identifier used in commands (`/conductor:*`)
- `version`: Semantic version of the extension
- `contextFileName`: File that provides context to Gemini CLI

## Conductor Methodology

When users install this extension and run commands, Conductor creates these artifacts in their projects:

```
conductor/
├── product.md              # Product vision and goals
├── product-guidelines.md   # Brand/style guidelines
├── tech-stack.md          # Technology decisions
├── workflow.md            # Development workflow
├── code_styleguides/      # Copied style guides
├── tracks.md              # Master list of all tracks
├── setup_state.json       # Setup progress state
└── tracks/
    └── <track_id>/
        ├── spec.md        # Feature/bug specification
        ├── plan.md        # Phased implementation plan
        └── metadata.json  # Track metadata
```

## Development Guidelines

### Making Changes to Commands

1. Command prompts are embedded in TOML files as multi-line strings
2. The prompts use markdown formatting with numbered sections
3. Key patterns used in prompts:
   - `## X.X SECTION NAME`: Major sections
   - `**PROTOCOL:**`: Defines a specific procedure
   - `**CRITICAL:**`: Highlights must-follow instructions
   - `**Action:**`: Specific action the AI should take

### Testing Changes

Since this is an extension for Gemini CLI, testing requires:
1. Installing the extension locally
2. Running commands in a test project
3. Verifying the AI follows the updated prompts correctly

### Code Style

- Use clear, imperative language in prompts
- Structure prompts as numbered steps for AI clarity
- Include error handling instructions (what to do when things fail)
- Provide examples for complex outputs (markdown, JSON structures)

## Important Considerations

### No Runtime Code

This extension has no traditional runtime code—it's entirely prompt-based. The "logic" lives in the TOML command prompts that guide AI behavior.

### User Project Context

The templates and prompts reference files that exist in **user projects**, not in this repository. For example:
- `conductor/tracks.md` exists in user projects after setup
- `conductor/workflow.md` is copied from `templates/workflow.md`

### Versioning

When making changes:
- Update the version in `gemini-extension.json`
- Document changes for users who have `--auto-update` enabled
- Consider backward compatibility with existing user projects

## Related Documentation

- `README.md`: User-facing documentation and installation guide
- `CONTRIBUTING.md`: How to contribute (CLA, code review process)
- `GEMINI.md`: Context file that helps Gemini CLI understand user queries about plans/tracks
