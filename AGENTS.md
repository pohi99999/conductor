# Agent Guidelines for Conductor Development

## How to Use This Document

This document is specifically designed for **AI agents and copilots** (such as Cursor, GitHub Copilot, Claude, etc.) working on the **Conductor browser extension codebase**. It ensures code quality, security, and UX consistency across all contributions.

**Quick Navigation:**
- [Purpose & Scope](#purpose--scope)
- [Codebase Overview](#codebase-overview)
- [Agent Responsibilities](#agent-responsibilities)
- [Style & Security Rules](#style--security-rules)
- [Tool Usage Protocol](#tool-usage-protocol)
- [Pull Request Checklist](#pull-request-checklist)
- [Known Limitations](#known-limitations)

---

## Purpose & Scope

**What is Conductor?**  
Conductor is a browser extension built on the **Gemini CLI** that enables **Context-Driven Development**. It helps developers plan, spec, and implement features using structured workflows defined in `commands/conductor/` and `templates/`.

**Document Purpose:**  
This guide provides MI agents with specific instructions to maintain consistency, avoid common pitfalls, and integrate smoothly with the project's workflow. It complements [CONTRIBUTING.md](CONTRIBUTING.md) (for human contributors) without duplicating content.

---

## Codebase Overview

For detailed repository structure, see [README.md](README.md). Key directories:

- **`commands/conductor/`**: TOML definitions for commands (`setup`, `newTrack`, `implement`, `status`, `revert`). Contains prompt logic for AI agents.
- **`templates/`**: Markdown templates for user artifacts (`workflow.md`, `code_styleguides/`).
- **`GEMINI.md`**: Context file for Gemini CLI.
- **`README.md`**: User-facing documentation.
- **`.github/workflows/`**: CI/CD for packaging and releases.

**✅ DO:**
- Read `README.md` and `GEMINI.md` before making changes
- Align changes with "Context-Driven Development" philosophy

**❌ DON'T:**
- Modify repository structure without discussing in an issue
- Change core workflows without updating all related documentation

---

## Agent Responsibilities

### File Permissions

**Read-Only Files** (require human approval for changes):
- `CONTRIBUTING.md`, `LICENSE`, `SECURITY.md`
- `.github/workflows/*` (CI/CD pipelines)
- Any files containing secrets or environment variables

**Editable Files** (agents can modify with standard PR process):
- `commands/conductor/*.toml`
- `templates/*.md`
- `README.md`, `GEMINI.md`
- Code files for features/bugfixes

**Human Review Required** for:
- Authentication/authorization code
- Payment processing
- Data handling (PII, sensitive information)
- Security-related changes
- Breaking changes to public APIs

**✅ DO:**
- Request human review when unsure about permissions
- Document why you need to modify sensitive files

**❌ DON'T:**
- Modify build pipelines without explicit ticket approval
- Change security configurations autonomously

---

## Style & Security Rules

### Code Style

Follow the principles in `templates/workflow.md`:

1. **Test-Driven Development (TDD)**: Write tests before implementation (where applicable)
2. **High Standards**: Ensure code coverage and follow style guides
3. **Conventional Commits**: Use format `type(scope): description`
   - Examples: `feat(cli): add new command`, `fix(implement): resolve TOML parsing`, `docs(readme): update installation steps`

**✅ DO:**
- Run linter before committing
- Write descriptive commit messages
- Keep functions small and focused

**❌ DON'T:**
- Skip tests for new features
- Use vague commit messages like "fix bug" or "update code"
- Introduce breaking changes without deprecation warnings

### Command Definitions (TOML Files)

The `commands/conductor/*.toml` files contain **prompt engineering as code**:

**✅ DO:**
- Treat the `prompt` field as executable code
- Ensure instructions are clear, unambiguous, and robust
- Test prompts with multiple edge cases
- Update `README.md` or `GEMINI.md` if user-facing behavior changes

**❌ DON'T:**
- Use ambiguous language in prompts
- Modify protocol in `implement.toml` without updating documentation
- Assume users will understand implicit instructions

### Templates

`templates/workflow.md` is the "brain" of Conductor for end users:

**✅ DO:**
- Recognize that changes here affect ALL extension users
- Maintain consistency with Conductor's philosophy
- Update related templates if one template changes

**❌ DON'T:**
- Make template changes without testing impact on workflow
- Remove sections without understanding their purpose

---

## Tool Usage Protocol

When using AI-powered development tools:

**✅ DO:**
- Use Cursor/Copilot for boilerplate and refactoring
- Verify all AI-generated code manually
- Run tests after AI-assisted changes

**❌ DON'T:**
- Blindly accept AI suggestions for security-critical code
- Let AI modify files in `.github/workflows/` without review
- Use AI to generate commit messages without reviewing them

---

## Pull Request Checklist

Before submitting a PR, ensure:

- [ ] **Tests**: All tests pass (`npm test` or equivalent)
- [ ] **Linter**: Code passes linting (`npm run lint`)
- [ ] **Commit Messages**: Follow conventional commit format
- [ ] **Documentation**: Updated `README.md`, `GEMINI.md`, or other docs if needed
- [ ] **Issue Reference**: PR links to relevant issue (e.g., `Closes #123`)
- [ ] **Changelog**: Updated if change is user-facing
- [ ] **No Secrets**: No API keys, tokens, or sensitive data committed
- [ ] **Breaking Changes**: Documented in PR description with migration guide

**For Human Reviewers:**  
Include this in PR template: *"I have read and followed the guidelines in AGENTS.md."*

---

## Known Limitations

AI agents **MUST NOT**:

1. **Modify repository secrets** (GitHub Actions secrets, environment variables)
2. **Change CI/CD configuration** without explicit approval from maintainers
3. **Access or modify** `.env` files, API keys, or credentials
4. **Bypass security checks** or disable linting/testing requirements
5. **Create new commands** in `commands/conductor/` without discussing in an issue first
6. **Remove or deprecate features** without a deprecation plan

**If you encounter these scenarios, stop and request human input.**

---

## System Prompt for Agents

Use this prompt in Cursor or other AI tools:

```
You are working on the Conductor browser extension repository. Follow these rules:

1. Read AGENTS.md before making changes
2. Only refactor/fix bugs unless the ticket explicitly requests a new feature
3. Use conventional commits (e.g., "feat(cli): add command")
4. Run tests and linter before committing
5. Request human review for security, auth, payment, or CI/CD changes
6. Never modify .github/workflows/ without approval
7. Treat TOML prompt strings as code—ensure clarity and robustness
8. Update documentation (README.md, GEMINI.md) if behavior changes

If unsure, ask for clarification before proceeding.
```

---

## Useful References

- **Gemini CLI Extensions**: [Documentation](https://geminicli.com/docs/extensions/)
- **Conductor Core Logic**: Defined in command handlers and TOML configurations
- **Contributing Guidelines**: See [CONTRIBUTING.md](CONTRIBUTING.md)
- **Code of Conduct**: [Google Open Source Community Guidelines](https://opensource.google/conduct/)
