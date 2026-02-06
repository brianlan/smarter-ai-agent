# AI Agent Commands & Subagents

This repository provides a **plan-build-validate workflow** for AI-assisted development.

---

## Overview

| Component | Purpose |
|-----------|---------|
| `/multi-step-plan` | 8-phase planning command that creates detailed implementation plans with validation criteria |
| `build-then-validate` | Build agent that implements plans with automatic validation after each change |
| `validator` | Read-only subagent that validates implementation against acceptance criteria |
| `code-simplifier` | Refactoring agent for improving code quality (credit: nicknisi) |

---

## Quick Start

### Installation

```bash
# Clone this repository
git clone <repo-url> ~/ai-agent-config
cd ~/ai-agent-config

# For Claude Code - Symlink to global config
mkdir -p ~/.claude/commands ~/.claude/agents
ln -sf "$(pwd)/commands/multi-step-plan.md" ~/.claude/commands/multi-step-plan.md
ln -sf "$(pwd)/agents/build-then-validate.md" ~/.claude/agents/build-then-validate.md
ln -sf "$(pwd)/agents/validator.md" ~/.claude/agents/validator.md
ln -sf "$(pwd)/agents/code-simplifier.md" ~/.claude/agents/code-simplifier.md

# For Open Code - Copy to config directory
mkdir -p ~/.config/opencode/agents
cp agents/*.md ~/.config/opencode/agents/
```

---

## Usage

### /multi-step-plan

Creates detailed implementation plans through an 8-phase iterative process.

```
/multi-step-plan <your requirement>
```

**Phases:** Restate & Scope → Investigation → Clarifying Questions → Research Loop → Final Questions → Validation Criteria → Execution Plan → Save Artifacts

**Output:** Saves to `./plan-build-validate/sessions/<name>/`
- `user_requirements.md`
- `validation_criteria.md`
- `implementation_plan.md`

---

### build-then-validate

Implements plans created by `/multi-step-plan` with automatic validation.

```
@build-then-validate plan-build-validate/sessions/<name>/
```

**Requirements:** Must have run `/multi-step-plan` first to create session artifacts.

**Workflow:** Reads plan → Implements → Validates with @validator → Fixes if needed → Repeats until complete.
