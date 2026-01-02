# Spec-Driven Development (SDD) Guide for Claude Code

This guide outlines the methodology for Spec-Driven Development (SDD) using **Claude Code**. It leverages Claude's unique capabilities (hooks, slash commands, and context management) to enforce a disciplined, evidence-based engineering workflow.

## 1. Core Principles

1.  **User-Centric**: Every feature starts with a user story defining value (`user-story.md`).
2.  **Evidence-Based**: Technical decisions must be backed by citations and measurements.
3.  **Small Batch Sizes**: Work is broken down into small, independently value-delivering units.
4.  **TDD Mandatory**: The workflow enforces Red → Green → Refactor via pre-commit hooks.
5.  **Sequential Phases**: Strict ordering of User Story → Design → Task Breakdown → Implementation.

## 2. Directory Structure

The project structure is designed to be "machine-readable" by Claude Code hooks and commands:

```text
root/
├── .work-items/                 # 📋 CANONICAL SOURCE
│   └── {feature-name}/          # One directory per feature
│       ├── user-story.md        # Phase 1: User requirements (EARS format)
│       ├── design.md            # Phase 2: Technical design
│       ├── task.md              # Phase 3: Task breakdown
│       └── 01_step_name.md      # Phase 3+: Sequential implementation steps
│
├── .claude/                     # ⚙️ CLAUDE CODE CONFIGURATION
│   ├── hooks/                   # Automation Scripts
│   │   ├── pre_prompt.py        # Injects rules/standards into context
│   │   └── pre_commit.py        # Enforces passing tests before git commit
│   ├── commands/                # Custom Slash Commands
│   │   ├── start-feature.md     # Setup active work
│   │   └── complete-feature.md  # Verification & cleanup
│   ├── plans/                   # 📊 Active Work Tracking
│   │   ├── README.md            # Logic explanation
│   │   └── {feature}.plan.md    # Symlink to .work-items/{feature}/task.md
│   ├── skills/                  # Specialized knowledge (TDD, Architecture)
│   └── DEVELOPMENT.md           # Project context (auto-loaded)
│
├── rules/                       # 📚 Standards (injected by pre_prompt.py)
│   ├── process-core.mdc         # Core principles
│   ├── standards-user-story.mdc # User story template
│   └── ...                      # Other standards
│
└── tests/                       # ✅ Automated Tests (required by pre_commit)
```

## 3. Automation Features

### Hooks (`.claude/hooks/`)
*   **`pre_prompt.py`**: automatically inspects your prompt. If it detects you are in a coding session, it injects the content of `rules/*.mdc` and `DEVELOPMENT.md`. This ensures Claude "knows" the rules without you needing to repeat them.
*   **`pre_commit.py`**: running `git commit` triggers this hook. It executes `pytest`. If tests fail, the commit is **blocked**. This strictly enforces the "Green" state of TDD.

### Active Plans (`.claude/plans/`)
*   **Mechanism**: A symbolic link (symlink) is created in this folder pointing to the specific `.work-items/{feature}/task.md` currently being worked on.
*   **Purpose**: This provides a clear, git-tracked signal of "what is active".
*   **Discovery**: Claude checks this folder to know what its current focus should be.

## 4. The Workflow

### Step 1: Define the Work
Create the directory `.work-items/{feature-name}/` and populate:
1.  **`user-story.md`**: Define the Persona, Story, and Acceptance Criteria (EARS).
2.  **`design.md`**: Define the technical approach, API changes, and data models.
3.  **`task.md`**: Break the work into 1-4 hour chunks.
4.  **Step Files** (e.g., `01_scaffold.md`): Detailed instructions for each step.

### Step 2: Start the Feature (`/start-feature`)
Run `/start-feature {feature-name}`. This command:
1.  Validates the `.work-items` directory exists.
2.  **Creates a symlink** in `.claude/plans/` pointing to the task file.
3.  Commits this change to git (signaling work has started).
4.  Reads the context (`user-story`, `design`) into the session.
5.  Displays the first step to get you started.

### Step 3: TDD Loop (`/next-step`)
For each step in your task breakdown:
1.  **RED**: Write a failing test. Run `pytest` to confirm failure.
2.  **GREEN**: Write minimal code to pass the test. Run `pytest` to confirm success.
3.  **REFACTOR**: Clean up.
4.  **COMMIT**: When you `git commit`, `pre_commit.py` runs tests automatically. If they fail, you cannot commit.

*Tip: Use time tracking in your commits to monitor velocity vs assignments.*

### Step 4: Complete the Feature (`/complete-feature`)
Run `/complete-feature {feature-name}`. This command:
1.  **Verifies** all steps are done.
2.  **Checks** all acceptance criteria.
3.  **Runs** the full test suite (regression check).
4.  **Removes the symlink** from `.claude/plans/` (signaling work is done).
5.  **Commits** the completion with a detailed summary report (steps, tests passed, time saved).

## 5. Setup Instructions (New Project)

To enable this workflow in a new project:

1.  **Create Directories**:
    ```bash
    mkdir -p .claude/{hooks,commands,plans,skills} .work-items rules
    ```
2.  **Install Hooks**:
    *   Copy `pre_prompt.py` to `.claude/hooks/` to auto-load rules.
    *   Copy `pre_commit.py` to `.claude/hooks/` to enforce tests.
3.  **Install Commands**:
    *   Add `start-feature.md` and `complete-feature.md` to `.claude/commands/`.
4.  **Define Rules**:
    *   Place your `.mdc` standard files in `rules/`.
    *   Ensure `pre_prompt.py` references them.
5.  **Initialize Git**:
    *   `git init`
    *   ensure `.claude/plans` is committed (but empty except for a README).

## 6. Commit Message Standard

Claude Code generates structured commit messages to track progress:

```text
feat: implement vector search endpoint

[Details of changes]

⏱️ Time Tracking:
- Estimated: 2 hours
- Actual: 45 minutes
- Time saved: 1.25 hours

🤖 Generated with Claude Code
```

This data allows for retrospective analysis of estimation accuracy and velocity.
