# Gemini CLI Reference

## Overview

Gemini CLI provides access to Google's Gemini models for code generation, analysis, and refactoring with powerful features like 1M token context window and built-in Google Search grounding.

## Model Options

- **gemini-2.5-pro**: Most capable model, 1M token context window
- **gemini-2.5-flash**: Optimized for speed and efficiency

## Key Features

- 🎯 **Free tier**: 60 requests/min and 1,000 requests/day
- 🧠 **Large context**: 1M token context window with gemini-2.5-pro
- 🔧 **Built-in tools**: Google Search grounding, file operations, shell commands
- 🔌 **MCP support**: Model Context Protocol for custom integrations
- 💻 **Terminal-first**: Designed for command-line workflows

## Execution Modes

### Interactive Mode
For exploratory tasks, debugging, and multi-turn conversations:
```bash
gemini -m gemini-2.5-pro
```

**When to use**:
- Debugging sessions
- Exploratory code analysis
- Multi-step problem solving
- Iterative development

**Interactive commands**:
- `/help`: Show available commands
- `/chat`: Start a new conversation
- `/exit`: Exit the session

### Script Mode
For one-shot tasks with direct prompts:
```bash
gemini -m gemini-2.5-flash -p "your prompt here"
```

**When to use**:
- Quick code reviews
- Automated workflows
- Single-task executions
- CI/CD integrations

## Command Options

### Model Selection
```bash
-m, --model <MODEL>
```
Choose between gemini-2.5-pro and gemini-2.5-flash

### Direct Prompt
```bash
-p, --prompt <PROMPT>
```
Provide a prompt directly for script mode

### Directory Inclusion
```bash
--include-directories <DIRS>
```
Include specific directories in context (comma-separated)
Example: `--include-directories ./src,./lib,./tests`

### Output Format
```bash
--output-format json
```
Get structured JSON output for parsing and automation

### Working Directory
```bash
-C, --cwd <DIR>
```
Change working directory before running

## Running a Task

1. **Ask the user** which model to use:
   - gemini-2.5-pro: For complex analysis and large codebases
   - gemini-2.5-flash: For quick tasks and fast iterations

2. **Determine execution mode**:
   - Interactive: For exploration and debugging
   - Script: For specific tasks and automation

3. **Assemble the command**:

**Interactive mode**:
```bash
gemini -m <model> [--include-directories <dirs>] [-C <dir>]
```

**Script mode**:
```bash
gemini -m <model> -p "<prompt>" [--include-directories <dirs>] [--output-format json] [-C <dir>]
```

4. **Run and capture output**: Execute the command and summarize results

5. **Inform user**: Let them know they can continue or make additional changes

## Command Examples

### Interactive Code Analysis
```bash
gemini -m gemini-2.5-pro
```
Then interact in the session:
```
> Analyze the authentication module for security issues
> Explain the database schema
> /exit
```

### Script Mode Code Review
```bash
gemini -m gemini-2.5-flash -p "Review this code for potential bugs and suggest improvements"
```

### Large Codebase Analysis
```bash
gemini --include-directories ./src,./lib,./tests -m gemini-2.5-pro
```

### Structured Output for Automation
```bash
gemini -m gemini-2.5-flash -p "List all API endpoints in this codebase" --output-format json
```

### Code Refactoring
```bash
gemini -m gemini-2.5-flash -p "Refactor the user authentication to use async/await pattern"
```

### Different Directory
```bash
gemini -C /path/to/project -m gemini-2.5-pro -p "Explain the architecture"
```

### Bug Fixing
```bash
gemini --include-directories ./src -m gemini-2.5-pro -p "Find and fix the memory leak in the connection pool"
```

## Model Selection Guidelines

### Use gemini-2.5-pro when:
- Working with large codebases (up to 1M tokens)
- Need deep architectural analysis
- Complex refactoring tasks
- Generating extensive code
- Comprehensive security audits

### Use gemini-2.5-flash when:
- Quick code reviews
- Simple refactoring
- Fast iterations needed
- Smaller scope tasks
- Rapid prototyping

## Quick Reference Table

| Use case | Model | Mode | Example |
| --- | --- | --- | --- |
| Interactive debugging | pro | Interactive | `gemini -m gemini-2.5-pro` |
| Quick review | flash | Script | `gemini -m gemini-2.5-flash -p "review this"` |
| Large codebase | pro | Interactive | `gemini --include-directories ./src,./lib -m gemini-2.5-pro` |
| Automation | flash | Script | `gemini -p "analyze" --output-format json` |
| Complex refactor | pro | Script | `gemini -m gemini-2.5-pro -p "refactor to clean architecture"` |

## Authentication Setup

Before first use, ensure authentication is configured:

### Option 1: Google Account (Recommended)
```bash
gemini
# Select "Login with Google" when prompted
```

**Benefits**:
- Free tier: 60 requests/min, 1,000 requests/day
- Access to Gemini 2.5 Pro with 1M context window
- No API key management

### Option 2: API Key
```bash
export GEMINI_API_KEY="your-api-key"
gemini
```

Get your key from: https://aistudio.google.com/apikey

**Benefits**:
- Model-specific control
- Usage-based billing
- Up to 100 requests/day on free tier

### Option 3: Vertex AI (Enterprise)
```bash
export GOOGLE_API_KEY="your-api-key"
export GOOGLE_GENAI_USE_VERTEXAI=true
gemini
```

**Benefits**:
- Enterprise features
- Higher rate limits
- Google Cloud integration

## Following Up

After every Gemini command:
1. Confirm if the user wants to make additional changes
2. For complex tasks, suggest using interactive mode
3. If output needs adjustment, run another command with refined instructions
4. Mention that they can continue in interactive mode for iterative work

## Error Handling

### Installation Issues
If `gemini` is not installed:
- `npm install -g @google/gemini-cli`
- `brew install gemini-cli`
- Or use without installation: `npx @google/gemini-cli`

### Authentication Failures
- Run `gemini` and choose "Login with Google"
- Or set `GEMINI_API_KEY` environment variable
- Or configure Vertex AI with `GOOGLE_API_KEY` and `GOOGLE_GENAI_USE_VERTEXAI=true`

### Rate Limit Errors
- Free tier: 60 requests/min, 1,000 requests/day
- Suggest spacing out requests
- Recommend upgrading to paid tier for higher limits

### Version Check
If encountering errors:
```bash
gemini --version
```

## Best Practices

1. **Directory inclusion**: Use `--include-directories` to focus on relevant code and reduce token usage
2. **Model selection**: Start with flash for quick tasks, use pro for complex analysis
3. **Structured output**: Use `--output-format json` when integrating with scripts or pipelines
4. **Context management**: For large projects, break tasks into smaller, focused queries
5. **Interactive sessions**: Use interactive mode for debugging and iterative development
6. **Token optimization**: Include only necessary directories to stay within context limits
7. **Free tier management**: Monitor request counts to stay within daily limits

## Advanced Features

### Google Search Grounding
Gemini CLI includes built-in Google Search for real-time information:
```bash
gemini -m gemini-2.5-pro -p "What are the latest best practices for React 19?"
```

### MCP Server Integration
Configure MCP servers in `~/.gemini/settings.json` to extend capabilities:
```bash
> @github List my open pull requests
> @slack Send summary to #dev channel
```

### Conversation Checkpointing
Save and resume complex sessions across multiple runs

### Custom Context Files
Use GEMINI.md files to tailor behavior for specific projects

## Comparison with Codex CLI

**Choose Gemini when**:
- Need large context window (1M tokens)
- Want Google Search integration
- Prefer interactive debugging
- Working with massive codebases

**Choose Codex when**:
- Need precise reasoning effort control
- Want session resume capability
- Require specific sandbox modes
- Need OpenAI's latest models
