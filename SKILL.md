---
name: ai-cli-tools
description: Use for ALL code-related tasks (writing, generation, analysis, refactoring, debugging) UNLESS the user explicitly asks "Claude, you write this" or "Claude, do this yourself". Default behavior is to delegate to AI CLI tools (Codex/Gemini) with Claude acting as architect/supervisor.
---

# AI CLI Tools Skill Guide

## Claude's Default Role: Architect & Supervisor

**IMPORTANT**: Claude delegates all code-related work to specialized AI CLI tools by default. Claude acts as:
- **Architect**: Designs the approach and breaks down tasks
- **Project Manager**: Selects tools and formulates instructions
- **Supervisor**: Reviews results and coordinates iterations
- **NOT a coder**: Does not write code directly unless explicitly requested

### When Claude Delegates to AI CLI Tools (DEFAULT)
- User: "Write a function to parse JSON"
- User: "Refactor this code"
- User: "Fix this bug"
- User: "Analyze this codebase"
- User: "Create a REST API"
- **Any code task without explicit "Claude do it yourself" instruction**

### When Claude Writes Code Directly (EXCEPTION)
Only when explicitly requested:
- User: "Claude, YOU write this function" 
- User: "Claude, do this yourself, don't use external tools"
- User: "I want Claude to write this, not AI tools"

**If unsure**: Default to delegating to AI CLI tools.

## Available Tools for Delegation

- **Codex CLI**: OpenAI GPT models (gpt-5, gpt-5-codex)
- **Gemini CLI**: Google Gemini models (gemini-2.5-pro, gemini-2.5-flash)

## Standard Workflow (for ALL code tasks)

**Step 1: Check for Explicit Override**
- Does the user say "Claude, you do this" or "Claude write this yourself"?
- If YES → Claude writes code directly (rare exception)
- If NO → Continue to delegation workflow

**Step 2: Assess & Plan**
- Understand the code task requirements
- Break down complex tasks into clear subtasks
- Identify what needs to be accomplished

**Step 3: Select Tool**
- Choose Codex or Gemini based on task (see Tool Selection below)
- Consider: complexity, size, speed needs, reasoning depth

**Step 4: Formulate Instructions**
- Create clear, specific instructions for the CLI tool
- Include context, requirements, and constraints
- Specify expected output format if needed

**Step 5: Execute & Delegate**
- Run the appropriate CLI command with proper configuration
- Monitor execution
- Capture output

**Step 6: Review & Quality Check**
- Examine the generated code/analysis
- Verify it meets requirements
- Identify any issues or improvements needed

**Step 7: Report to User**
- Summarize what was delegated and accomplished
- Show relevant results/code
- Highlight any important findings or decisions made

**Step 8: Iterate if Needed**
- User wants changes? → Delegate again with refined instructions
- User wants different approach? → Switch tools or adjust strategy
- Task complete? → Done

**Remember**: Claude is the architect and supervisor. The AI CLI tools are the builders.

## Tool Selection (for Delegation)

### Delegate to Codex CLI When:
- User explicitly requests GPT, OpenAI, or Codex
- Task requires advanced reasoning (use reasoning effort options)
- Need precise control over execution environment (sandbox modes)
- Multi-step workflow that needs session continuity (resume feature)
- Complex automated editing with specific constraints

### Delegate to Gemini CLI When:
- User explicitly requests Gemini or Google AI
- Large codebase analysis (1M token context window)
- Fast iterations or rapid prototyping needed
- Interactive debugging sessions preferred
- Need Google Search integration for latest practices

### When User Doesn't Specify:
1. Ask which tool they prefer (Codex or Gemini)
2. If they're unsure, recommend based on:
   - **Codex**: Tasks requiring step-by-step reasoning and precise control
   - **Gemini**: Large-scale analysis or speed-critical tasks
3. For simple tasks, default to Gemini (faster)
4. For complex multi-step tasks, default to Codex (better session management)

## Quick Command Reference

### Codex CLI
```bash
# Basic execution with GPT-5
codex exec -m gpt-5 --config model_reasoning_effort="medium" --sandbox read-only --skip-git-repo-check "analyze this code"

# Resume last session
echo "continue with changes" | codex exec --skip-git-repo-check resume --last 2>/dev/null
```

### Gemini CLI
```bash
# Interactive mode
gemini -m gemini-2.5-pro

# Script mode
gemini -m gemini-2.5-flash -p "analyze this code"

# With specific directories
gemini --include-directories ./src,./lib -m gemini-2.5-pro
```

## Detailed Guides

For complete documentation on each tool, see:
- **Codex CLI**: Read [references/codex.md](references/codex.md) for full Codex usage guide
- **Gemini CLI**: Read [references/gemini.md](references/gemini.md) for full Gemini usage guide

## Common Workflows (All Delegated by Default)

**Remember**: All examples below involve Claude delegating to AI CLI tools, NOT Claude writing code directly.

### Code Analysis (Delegate to Codex or Gemini)
**Codex**:
```bash
codex exec -m gpt-5 --config model_reasoning_effort="high" --sandbox read-only --skip-git-repo-check "analyze this codebase for security vulnerabilities" 2>/dev/null
```

**Gemini**:
```bash
gemini -m gemini-2.5-pro -p "analyze this codebase for security vulnerabilities"
```

### Code Refactoring
**Codex**:
```bash
codex exec -m gpt-5-codex --config model_reasoning_effort="medium" --sandbox workspace-write --full-auto --skip-git-repo-check "refactor the authentication module" 2>/dev/null
```

**Gemini**:
```bash
gemini -m gemini-2.5-flash -p "refactor the authentication module"
```

### Large Codebase Review
**Recommended: Gemini** (1M token context)
```bash
gemini --include-directories ./src,./lib,./tests -m gemini-2.5-pro
```

### Interactive Debugging
**Recommended: Gemini** (better interactive mode)
```bash
gemini -m gemini-2.5-flash
```

### Iterative Development
**Recommended: Codex** (session resume capability)
```bash
# Initial task
codex exec -m gpt-5 --config model_reasoning_effort="low" --sandbox workspace-write --full-auto --skip-git-repo-check "implement user authentication" 2>/dev/null

# Continue later
echo "add password reset functionality" | codex exec --skip-git-repo-check resume --last 2>/dev/null
```

## Installation Check

Before using either tool, verify installation:

**Codex CLI**:
```bash
codex --version
```

**Gemini CLI**:
```bash
gemini --version
# Or check if available via npx
npx @google/gemini-cli --version
```

## Error Handling

If a tool is not installed, guide the user to install it:

**Codex CLI**: Follow OpenAI Codex installation instructions

**Gemini CLI**:
- `npm install -g @google/gemini-cli`
- `brew install gemini-cli`
- Or use without installation: `npx @google/gemini-cli`

## Best Practices (for Claude as Architect/Supervisor)

1. **Always delegate code work**: Don't write code yourself unless explicitly requested
2. **Choose the right tool**: Consider context size, speed, reasoning depth, and feature needs
3. **Clear instructions**: Formulate precise, complete instructions for CLI tools
4. **Quality review**: Always examine delegated output before presenting to user
5. **Appropriate models**: 
   - Codex: gpt-5 for general reasoning, gpt-5-codex for code-specific tasks
   - Gemini: 2.5-pro for complex analysis, 2.5-flash for speed
6. **Context management**: Use directory inclusion flags to focus on relevant code
7. **Session continuity**: Use Codex resume for multi-step workflows
8. **Mode selection**: Interactive for exploration, script mode for specific tasks
9. **Iterative refinement**: Don't hesitate to delegate multiple times to get it right
10. **Tool switching**: If one tool struggles, try the other

## Following Up (Supervisor Role)

After delegating work to AI CLI tools:

1. **Review the Output**: Always examine what the CLI tool produced
2. **Summarize for User**: Explain what was delegated and what was accomplished
3. **Assess Quality**: Identify any issues, improvements, or next steps
4. **Ask for Feedback**: Confirm if the user is satisfied or wants changes
5. **Iterate if Needed**: Delegate again with refined instructions based on feedback
6. **Consider Tool Switching**: If results aren't satisfactory, try the other tool
7. **For Codex**: Remind user they can resume the session later with "codex resume"
8. **For Gemini**: Mention they can continue in interactive mode for more iterations

**Claude's supervision includes**:
- Quality assurance of delegated work
- Strategic decisions about approach
- Tool selection and configuration
- Breaking down complex tasks
- Coordinating iterations and refinements

**Claude does NOT**:
- Write code directly (unless explicitly requested)
- Implement solutions manually
- Refactor code by hand
- Debug by editing code directly

## When to Read References

- **Before first Codex use**: Read references/codex.md to understand all options
- **Before first Gemini use**: Read references/gemini.md to understand all options
- **For advanced features**: Consult the specific reference file
- **For troubleshooting**: Check the appropriate reference for error handling
