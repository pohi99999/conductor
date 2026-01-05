# Agent Guidelines for Conductor Development

This file provides instructions and context for AI agents working on the **Conductor** extension repository.

## Project Purpose

Conductor is a Gemini CLI extension that enables **Context-Driven Development**. It helps developers plan, spec, and implement features using a structured workflow. The extension itself is defined by the command configurations in `commands/conductor/` and the templates in `templates/`.

## Repository Structure

-   `commands/conductor/`: TOML definitions for Conductor commands (`setup`, `newTrack`, `implement`, `status`, `revert`). These files contain the prompt logic for the AI agents that end-users will interact with.
-   `templates/`: Markdown templates used to generate user project artifacts (e.g., `workflow.md`, `code_styleguides/`).
-   `GEMINI.md`: Context file for the Gemini CLI to understand this extension.
-   `README.md`: User-facing documentation.
-   `.github/workflows/`: CI/CD workflows for packaging and releasing the extension.

## Guidelines for AI Agents

When acting as a developer on this repository, please adhere to the following guidelines:

### 1. Understand the Product
Read `README.md` and `GEMINI.md` to understand how Conductor is supposed to work. Your changes should align with the core philosophy of "Context-Driven Development".

### 2. Respect the Workflow
When writing code for this repository, follow the principles outlined in `templates/workflow.md`, specifically:
-   **Test-Driven Development (TDD)**: Write tests before implementation (where applicable/possible for this extension format).
-   **High Standards**: Ensure code coverage and follow style guides.
-   **Commit Messages**: Use the conventional commit format (e.g., `feat(cli): add new command`).

### 3. Command Definitions
The core logic of Conductor is often embedded in the prompts within `commands/conductor/*.toml`.
-   **Prompt Engineering**: When modifying these TOML files, treat the `prompt` string as code. Ensure instructions are clear, unambiguous, and robust.
-   **Protocol**: If you modify the protocol in `implement.toml`, ensure you update the corresponding documentation in `README.md` or `GEMINI.md` if the user-facing behavior changes.

### 4. Templates
If you change the behavior of Conductor, check if the templates in `templates/` need to be updated to reflect the new behavior.
-   `templates/workflow.md`: This is the "brain" of the Conductor process for the user. Changes here ripple out to all users of the extension.

### 5. Documentation
Keep `README.md` and `GEMINI.md` in sync with any functional changes. If you add a new command, it must be documented.

## Useful References

-   **Gemini CLI Extensions**: [Documentation](https://geminicli.com/docs/extensions/)
-   **Conductor Logic**: The core logic is defined in the command handlers and the TOML configurations.
