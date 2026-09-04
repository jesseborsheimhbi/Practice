# Practice

A sandbox repository for practicing Claude Code workflows.

## Sub agents

Custom sub agents live in `.claude/agents/`. Each file is a YAML frontmatter block
(name, description, tools, model) followed by the agent's system prompt. Claude Code
loads them automatically when you open this repository.

| Agent | Purpose | Tools |
| --- | --- | --- |
| `code-reviewer` | Reviews diffs and changed files for bugs, security issues, and style problems. Read-only. | Read, Grep, Glob, Bash |
| `test-writer` | Writes or extends unit tests that follow the project's existing conventions, runs them, and fixes failures. | Read, Grep, Glob, Bash, Write, Edit |
| `docs-writer` | Writes or updates README sections, docstrings, and developer docs for existing code. Never edits code. | Read, Grep, Glob, Write, Edit |

### Launching a sub agent

Ask for the agent by name in a normal prompt:

```text
Use the code-reviewer agent to review my uncommitted changes.
Use the test-writer agent to add tests for src/parser.py.
Use the docs-writer agent to document the CLI flags in README.md.
```

Claude will also delegate to these agents on its own when a task matches an agent's
description, for example running `code-reviewer` after making code changes.

### Launching several sub agents at once

Independent tasks can run in parallel. Ask for them in one prompt:

```text
In parallel: have test-writer add tests for src/parser.py, and have docs-writer
document the public functions in src/parser.py.
```

Each sub agent runs in its own context window and reports back a summary; only that
summary returns to the main conversation, which keeps the main context small.

### Managing agents

- Run `/agents` inside Claude Code to list, create, or edit sub agents interactively.
- Project agents in `.claude/agents/` take precedence over user-level agents in
  `~/.claude/agents/` when names collide.
- Leave out the `tools` field to give an agent every tool the main session has;
  set `model` to `sonnet`, `opus`, or `haiku` to pin a model instead of inheriting.
