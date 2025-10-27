# Codex CLI Reference

## Overview

Codex CLI provides access to OpenAI's GPT models for code generation, analysis, and automated editing.

## Model Options

- **gpt-5**: Latest general-purpose model
- **gpt-5-codex**: Optimized for code-specific tasks

## Reasoning Effort Levels

- **low**: Faster responses, less deep analysis
- **medium**: Balanced speed and quality (recommended default)
- **high**: Most thorough analysis, slower responses

## Sandbox Modes

### read-only
- **Use for**: Code review, analysis, documentation
- **Permissions**: Read files only, no modifications
- **Safety**: Safest option, no changes to codebase

### workspace-write
- **Use for**: Local file modifications, refactoring
- **Permissions**: Read and write within workspace
- **Safety**: Moderate, changes limited to workspace

### danger-full-access
- **Use for**: Network operations, system-wide changes
- **Permissions**: Full system access
- **Safety**: Use with caution, ask user permission first

## Running a Task

1. **Ask the user** which model to run and reasoning effort in a single prompt:
   - Model: gpt-5 or gpt-5-codex
   - Reasoning effort: low, medium, or high

2. **Select sandbox mode** based on task requirements:
   - Default to `read-only` unless edits are needed
   - Use `workspace-write` for local file changes
   - Only use `danger-full-access` with explicit user permission

3. **Assemble the command**:
```bash
codex exec \
  -m <model> \
  --config model_reasoning_effort="<low|medium|high>" \
  --sandbox <read-only|workspace-write|danger-full-access> \
  --skip-git-repo-check \
  [--full-auto] \
  [-C <directory>] \
  "<prompt>" \
  2>/dev/null
```

4. **Important flags**:
   - `--skip-git-repo-check`: Always use this flag (required)
   - `--full-auto`: Use with workspace-write or danger-full-access for automatic execution
   - `-C <DIR>`: Change directory before running
   - `2>/dev/null`: Suppress thinking tokens (remove only if debugging)

5. **Run and capture output**: Execute the command and summarize results

6. **Inform user about resume**: Tell them they can resume with "codex resume" or continue the session later

## Session Resume

To continue a previous Codex session:

```bash
echo "new prompt here" | codex exec --skip-git-repo-check resume --last 2>/dev/null
```

**Important notes**:
- Do not include model, reasoning effort, or sandbox flags when resuming
- The resumed session inherits all settings from the original session
- All configuration flags must go between `exec` and `resume`
- Always pipe the prompt via stdin with `echo`

## Command Examples

### Read-only Code Analysis
```bash
codex exec -m gpt-5 --config model_reasoning_effort="medium" --sandbox read-only --skip-git-repo-check "analyze this code for bugs" 2>/dev/null
```

### Local File Refactoring
```bash
codex exec -m gpt-5-codex --config model_reasoning_effort="low" --sandbox workspace-write --full-auto --skip-git-repo-check "refactor this function" 2>/dev/null
```

### Deep Architectural Analysis
```bash
codex exec -m gpt-5 --config model_reasoning_effort="high" --sandbox read-only --skip-git-repo-check "explain the system architecture" 2>/dev/null
```

### Different Directory
```bash
codex exec -m gpt-5 --config model_reasoning_effort="medium" --sandbox read-only -C /path/to/project --skip-git-repo-check "review the codebase" 2>/dev/null
```

### Resume Session
```bash
echo "now add error handling" | codex exec --skip-git-repo-check resume --last 2>/dev/null
```

## Quick Reference Table

| Use case | Model | Reasoning | Sandbox | Key flags |
| --- | --- | --- | --- | --- |
| Code review | gpt-5 | medium | read-only | `--skip-git-repo-check 2>/dev/null` |
| Quick refactor | gpt-5-codex | low | workspace-write | `--full-auto --skip-git-repo-check 2>/dev/null` |
| Deep analysis | gpt-5 | high | read-only | `--skip-git-repo-check 2>/dev/null` |
| System changes | gpt-5 | medium | danger-full-access | `--full-auto --skip-git-repo-check 2>/dev/null` |
| Resume session | (inherited) | (inherited) | (inherited) | `resume --last 2>/dev/null` |

## Following Up

After every Codex command:
1. Use `AskUserQuestion` to confirm next steps
2. Ask if they want to resume, make changes, or try a different approach
3. Restate the model, reasoning effort, and sandbox mode when proposing follow-ups
4. Remind them they can resume the session anytime

## Error Handling

**Command exits non-zero**:
- Stop immediately and report the error
- Show the error message to the user
- Request direction before retrying

**High-impact flags**:
- Before using `--full-auto`, ask user permission
- Before using `--sandbox danger-full-access`, ask user permission
- Before using `--skip-git-repo-check`, mention it to user (or just use it as it's generally safe)

**Partial results or warnings**:
- Summarize what worked and what didn't
- Ask the user how to adjust using `AskUserQuestion`

**Installation issues**:
- Check if codex is installed: `codex --version`
- If not found, direct user to OpenAI Codex installation instructions

## stderr Handling

**Default behavior**: Suppress stderr with `2>/dev/null` to hide thinking tokens

**Show stderr only when**:
- User explicitly requests to see thinking tokens
- Debugging is needed
- Command fails and error details are required

## Best Practices

1. **Always use `--skip-git-repo-check`**: This flag is required for most operations
2. **Default to read-only**: Only escalate sandbox permissions when necessary
3. **Suppress thinking tokens**: Use `2>/dev/null` by default for cleaner output
4. **Ask before dangerous operations**: Get user confirmation for full-auto and danger-full-access
5. **Resume when appropriate**: Use session resume for multi-step workflows
6. **Match reasoning effort to task**: low for simple tasks, high for complex analysis
7. **Choose the right model**: gpt-5-codex for code-specific tasks, gpt-5 for general
