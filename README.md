# Superpowers (jimweller fork)

Fork of [obra/superpowers](https://github.com/obra/superpowers)
v5.0.7 with customizations for the jimweller dotfiles toolchain.

## Fork Changes

### beads (bd) integration

The plan execution pipeline uses bd (beads) for persistent task
tracking instead of TodoWrite.

- `writing-plans` creates a bd epic with child tasks and
  dependency ordering after writing the plan file
- `subagent-driven-development` uses `bd ready`,
  `bd update --claim`, `bd close --reason` for the task loop
- `executing-plans` uses the same bd lifecycle for inline
  execution
- TodoWrite retained for simple skill checklists in
  `using-superpowers` and `writing-skills`

Requires: bd CLI v0.60.0+ and an initialized `.beads/`
database in the project.

### /review-quick replaces code-quality-reviewer

`subagent-driven-development` uses `/review-quick` (8 parallel
specialized review agents) instead of the single
`./code-quality-reviewer-prompt.md` template for per-task code
quality review.

### test-driven-development: London School

The TDD skill enforces London School (mock-first) TDD.
Collaborators are mocked by default. The unit under test is
real. Includes failure-path branches in the red-green-refactor
cycle.

### BLOCKED escalation

`subagent-driven-development` adds a step to re-dispatch with
`superpowers:systematic-debugging` when a subagent is stuck on
a failure.

### Output paths

Plan and spec files write to `.llmtmp/plans/` and
`.llmtmp/specs/` instead of `docs/superpowers/plans/` and
`docs/superpowers/specs/`.

## Architecture

```text
skills/
  brainstorming/               # Idea-to-design skill
  writing-plans/               # Plan + bd epic/task creation
  subagent-driven-development/ # Task loop (bd + /review-quick)
  executing-plans/             # Inline execution fallback (bd)
  finishing-a-development-branch/ # Merge/PR/discard decision
  using-git-worktrees/         # Worktree setup
  test-driven-development/     # London School TDD
  systematic-debugging/        # 4-phase root cause process
  verification-before-completion/ # Verify before claiming done
  using-superpowers/           # Session entry point
  writing-skills/              # Skill authoring guide
hooks/
  session-start                # Injects using-superpowers context
agents/                        # Subagent definitions
commands/                      # Slash commands
tests/                         # Integration and skill tests
```

## Upstream

Tracking `obra/superpowers` main branch. To sync:

```bash
git remote add upstream https://github.com/obra/superpowers.git
git fetch upstream
git merge upstream/main
```

Resolve conflicts in fork-customized files:
`skills/test-driven-development/`,
`skills/subagent-driven-development/`,
`skills/executing-plans/`, `skills/writing-plans/`,
`skills/brainstorming/`.

## License

MIT (inherited from upstream).
