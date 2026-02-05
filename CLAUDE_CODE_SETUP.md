# Hillway Digital - Claude Code Configuration Guide

> Complete setup documentation for standardising Claude Code across Hillway Property Consultants

**Generated:** 5 February 2026
**For:** Hillway Digital Department
**Author:** Jamie Smyth

---

## Company Context

### About Hillway Property Consultants

- **Website:** https://www.hillwayco.uk
- **RICS Registration:** 900798
- **Headquarters:** Cubo, 38 Carver Street, Sheffield, S1 4FS
- **Secondary Office:** First Floor, David House, 30 South Parade, Bawtry, DN10 6JH
- **Founder:** Matt Fitzgerald BSc Hons, MRICS

### Core Services

1. **Property & Asset Management Consultancy** - Full-spectrum commercial portfolio management
2. **Service Charge Consultancy** - RICS-compliant budgeting and cost allocation
3. **Digital Transformation & PropTech Integration** - Cloud platforms and AI-driven analytics
4. **Facilities Management Advice** - H&S compliance, maintenance planning
5. **Lease Advisory** - Rent reviews, renewals, restructuring
6. **Strategic Property Management Advice** - Organizational development

### Hillway Digital (PropTech Division)

- AI-powered automation tools
- Cloud-based management platforms
- IoT solutions for property monitoring
- Advanced analytics transforming property data into strategic insights
- Change management and staff training for technology adoption

---

## Claude Code Installation

### Prerequisites

1. **macOS/Linux** (Windows via WSL2)
2. **Node.js 18+** installed
3. **Anthropic API key** or Claude Max subscription

### Installation

```bash
# Install Claude Code globally
npm install -g @anthropic-ai/claude-code

# Verify installation
claude --version

# First run - will prompt for authentication
claude
```

---

## Directory Structure

```
~/.claude/
├── CLAUDE.md              # Global instructions (loaded every session)
├── settings.json          # Global settings (model, hooks, permissions)
├── settings.local.json    # Local overrides (not committed)
├── commands/              # Custom slash commands
│   ├── commit.md
│   ├── pr.md
│   ├── review.md
│   └── ... (more below)
├── agents/                # Custom subagent definitions
│   └── residential-architect.md
├── plugins/               # Installed plugins
├── projects/              # Project-specific memory
│   └── -Users-jamiesmyth/
│       └── memory/
│           └── MEMORY.md  # Persistent memory across sessions
└── plans/                 # Saved implementation plans
```

---

## Global Configuration (settings.json)

```json
{
  "model": "claude-opus-4-5-20251101",
  "permissions": {
    "allow": [
      "WebSearch",
      "WebFetch"
    ],
    "deny": []
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "if [[ \"$CLAUDE_FILE_PATH\" == *.ts ]] || [[ \"$CLAUDE_FILE_PATH\" == *.tsx ]] || [[ \"$CLAUDE_FILE_PATH\" == *.js ]] || [[ \"$CLAUDE_FILE_PATH\" == *.jsx ]]; then npx prettier --write \"$CLAUDE_FILE_PATH\" 2>/dev/null || true; fi"
          }
        ]
      },
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "if [[ \"$CLAUDE_FILE_PATH\" == *.py ]]; then black \"$CLAUDE_FILE_PATH\" 2>/dev/null || true; fi"
          }
        ]
      }
    ]
  }
}
```

### Key Settings Explained

| Setting | Value | Purpose |
|---------|-------|---------|
| `model` | `claude-opus-4-5-20251101` | Use Opus 4.5 for complex reasoning |
| `permissions.allow` | `WebSearch`, `WebFetch` | Auto-approve web access |
| `hooks.PostToolUse` | Prettier/Black | Auto-format code after edits |

---

## CLAUDE.md - Global Instructions

This file is loaded into every Claude Code session. Create at `~/.claude/CLAUDE.md`:

```markdown
# Global Claude Code Configuration

## About Me
- Name: [Your Name]
- Focus: [Your focus areas]
- Languages: TypeScript/Node.js and Python
- Role: [Your role at Hillway]

## Default Development Standards

### TypeScript/JavaScript Projects
- Use TypeScript strict mode always
- Prefer `pnpm` over npm/yarn
- Use ES modules (`import`/`export`), not CommonJS
- Prefer `const` over `let`, never use `var`
- Use async/await over raw promises
- Handle errors explicitly - no silent catches
- Use Zod for runtime validation

### Python Projects
- Python 3.11+ with type hints everywhere
- Use `uv` or `poetry` for dependency management
- Follow PEP 8, use Black for formatting
- Use Pydantic for data validation
- Prefer `pathlib` over `os.path`
- Use `httpx` over `requests` for async support

### General Code Quality
- Write self-documenting code - minimize comments
- Keep functions small and single-purpose
- Prefer composition over inheritance
- Write tests for business logic
- No console.log/print statements in production code

## Git Conventions
- Use conventional commits: `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`
- Keep commits atomic and focused
- Branch naming: `feature/`, `fix/`, `refactor/`
- Always run tests before committing

## Testing Standards
- Test behavior, not implementation
- Use descriptive test names that explain the scenario
- Prefer integration tests over unit tests for APIs
- Mock external services, not internal modules

## Common Mistakes to Avoid
- Never use `any` type in TypeScript - use `unknown` if truly unknown
- Never store secrets in code - use environment variables
- Never ignore error handling - always handle or propagate
- Don't over-engineer - YAGNI (You Aren't Gonna Need It)
- Don't create abstractions until you have 3+ use cases

## When Working on AI/Agent Projects
- Structure prompts clearly with sections
- Always include error handling for LLM responses
- Implement retry logic with exponential backoff
- Log inputs/outputs for debugging
- Consider token limits in context management
- Use streaming for long responses

## Verification Steps
Before completing any task:
1. Run the test suite if it exists
2. Run linting/type checking
3. Verify the change works manually if possible
4. Check for any console errors or warnings

## Response Style Preferences
- Be direct and concise
- Show code, don't just describe it
- Explain trade-offs when relevant
- Challenge my assumptions if they seem wrong
```

---

## Custom Slash Commands

All commands go in `~/.claude/commands/`. Each `.md` file becomes a `/command`.

### Development Commands

#### /commit - Create Git Commits
**File:** `~/.claude/commands/commit.md`
```markdown
Create a git commit for the current changes.

1. Run `git status` and `git diff --staged` to see what's changed
2. If nothing is staged, stage all relevant changes (but not .env files or other secrets)
3. Write a conventional commit message:
   - Use format: `type(scope): description`
   - Types: feat, fix, docs, refactor, test, chore
   - Keep description under 72 characters
   - Add body if the change needs explanation
4. Create the commit
5. Show the result

Do not push unless I explicitly ask.
```

#### /pr - Create Pull Request
**File:** `~/.claude/commands/pr.md`
```markdown
Create a pull request for the current branch.

1. Check `git status` and ensure all changes are committed
2. Push the current branch to origin if not already pushed
3. Use `gh pr create` with:
   - Clear, descriptive title
   - Body with:
     - ## Summary (what and why)
     - ## Changes (bullet list of key changes)
     - ## Testing (how to verify)
4. Return the PR URL

If there are uncommitted changes, ask if I want to commit them first.
```

#### /review - Code Review
**File:** `~/.claude/commands/review.md`
```markdown
Review the code changes in this project.

Analyze recent changes using `git diff` (or `git diff HEAD~1` for last commit).

Check for:
1. **Logic errors**: Off-by-one, null checks, edge cases
2. **Security issues**: Injection, auth, data exposure
3. **Performance**: N+1 queries, unnecessary loops, memory leaks
4. **Code quality**: Readability, naming, duplication
5. **Testing**: Are changes tested? Are tests meaningful?
6. **Error handling**: Are errors caught and handled properly?

Format your review as:
- **Critical** (must fix): [issues]
- **Important** (should fix): [issues]
- **Suggestions** (nice to have): [issues]
- **Good** (what's done well): [positives]

Be specific with file names and line numbers.
```

#### /test - Run Tests
**File:** `~/.claude/commands/test.md`
```markdown
Run the test suite for this project.

1. Detect the project type and test framework:
   - Node/TS: look for jest, vitest, mocha in package.json
   - Python: look for pytest, unittest in pyproject.toml or requirements
2. Run the appropriate test command
3. If tests fail:
   - Analyze the failures
   - Suggest fixes
   - Ask if I want you to fix them
4. Report summary: passed, failed, skipped, coverage if available
```

#### /fix-issue - Fix GitHub Issue
**File:** `~/.claude/commands/fix-issue.md`
```markdown
Fix GitHub issue #$ARGUMENTS

1. Fetch the issue details using `gh issue view $ARGUMENTS`
2. Analyze the issue and understand what needs to be fixed
3. Search the codebase to find relevant files
4. Plan the fix (use plan mode if complex)
5. Implement the fix
6. Run tests to verify
7. Create a commit with message: `fix: [description] (closes #$ARGUMENTS)`

If the issue is unclear, ask me for clarification before starting.
```

#### /explain - Explain Code
**File:** `~/.claude/commands/explain.md`
```markdown
Explain the codebase or specific part: $ARGUMENTS

If $ARGUMENTS is empty, give a high-level overview:
1. Project structure and architecture
2. Key technologies and frameworks
3. Main entry points
4. How different parts connect

If $ARGUMENTS specifies a file or feature:
1. What it does and why it exists
2. How it works (step by step)
3. Key dependencies and relationships
4. Any gotchas or non-obvious behavior

Use diagrams (mermaid or ASCII) if it helps explain structure.
```

#### /refactor - Refactor Code
**File:** `~/.claude/commands/refactor.md`
```markdown
Refactor the specified code: $ARGUMENTS

1. Analyze the current implementation
2. Identify issues:
   - Code smells (duplication, long functions, etc.)
   - Complexity that can be simplified
   - Better patterns that could apply
3. Propose refactoring plan (use plan mode)
4. After approval, implement the refactoring
5. Run tests to ensure behavior is preserved
6. Show before/after comparison

Keep the same behavior - this is refactoring, not feature changes.
```

#### /deploy - Deployment Checklist
**File:** `~/.claude/commands/deploy.md`
```markdown
Prepare for deployment.

Pre-deployment checklist:
1. Run full test suite
2. Run linting and type checks
3. Check for console.log/print statements
4. Verify environment variables are documented
5. Check for hardcoded secrets or URLs
6. Review recent changes for risky code
7. Verify database migrations (if applicable)
8. Check dependencies for known vulnerabilities

Report:
- **Ready**: All checks passed
- **Warnings**: Non-blocking issues
- **Blockers**: Must fix before deploy

Do NOT actually deploy - just validate readiness.
```

#### /prime - Load Project Context
**File:** `~/.claude/commands/prime.md`
```markdown
Load project context and get ready to work.

1. Read the project's README.md and any CLAUDE.md files
2. Identify the tech stack from package.json, pyproject.toml, etc.
3. Understand the project structure (key directories, entry points)
4. Check for any .env.example to understand required config
5. Look at recent git commits to understand current work

Summarize:
- **Project**: What this project does
- **Stack**: Technologies used
- **Structure**: Key directories and their purpose
- **Entry points**: Where things start
- **Current state**: Recent activity and any WIP

Then say "Ready to help with [project name]. What would you like to work on?"
```

### AI/Agent Commands

#### /agent - Create or Improve AI Agent
**File:** `~/.claude/commands/agent.md`
```markdown
Create or improve an AI agent: $ARGUMENTS

For new agents:
1. Define the agent's purpose and scope
2. Design the system prompt with:
   - Clear role and constraints
   - Available tools/actions
   - Input/output formats
   - Error handling behavior
3. Implement the agent code with:
   - Proper error handling
   - Retry logic with backoff
   - Logging for debugging
   - Token management
4. Create test cases

For improving existing agents:
1. Analyze current behavior and issues
2. Identify prompt improvements
3. Optimize for reliability and consistency
4. Add missing error cases
5. Test with edge cases

Always consider: token limits, rate limits, cost, latency.
```

### Business Context Commands

#### /business - Business Analysis
**File:** `~/.claude/commands/business.md`
```markdown
Business analysis request: $ARGUMENTS

Analyze this from a strategic business perspective.

Structure your response:

## Summary
One-line recommendation or answer.

## Analysis
Key factors and considerations:
- Financial implications
- Strategic fit
- Market/competitive factors
- Resource requirements
- Risks and mitigations

## Recommendation
Specific, actionable next steps.

## Metrics
How to measure success/progress.

---

Be direct. Challenge assumptions. Use numbers where possible.
```

#### /hillway - Switch to Hillway Context
**File:** `~/.claude/commands/hillway.md`
```markdown
Switch context to Hillway AI.

Read the file ~/Business/HillwayAI/CLAUDE.md to load full context.

You are now advising on Hillway AI - PropTech and AI-powered commercial property consultancy.

Consider: technology innovation, property expertise, scalable solutions, client value, AI capabilities.

What would you like to discuss about Hillway AI?
```

#### /rent-review - Commercial Lease Advisor
**File:** `~/.claude/commands/rent-review.md`
```markdown
Switch context to Commercial Lease Advisor for rent reviews and lease renewals.

Read the file ~/Business/Workin/RentReviews/CLAUDE.md to load full context.

You are now the Commercial Lease Advisor - supporting rent reviews and lease renewals with RICS compliance.

Available commands:
- "Analyse this lease" - Review and summarise key provisions
- "Check VOA/EPC/planning for [address]" - External data lookup
- "Draft [document type]" - Generate from templates
- "Calculate deadlines for [date]" - Time limit calculator
- "Run due diligence on [address]" - Comprehensive property research

Templates available in ~/Business/Workin/RentReviews/templates/
Guidance docs in ~/Business/Workin/RentReviews/guidance/

What property matter would you like to work on?
```

#### /portfolio - Load All Business Context
**File:** `~/.claude/commands/portfolio.md`
```markdown
Load full business portfolio context.

Read all of these files to understand Jamie's complete business portfolio:
- ~/Business/Livin/CLAUDE.md
- ~/Business/Workin/CLAUDE.md
- ~/Business/JockSergisonGarage/CLAUDE.md
- ~/Business/HillwayAI/CLAUDE.md
- ~/Business/S80Partnership/CLAUDE.md
- ~/Business/StTheresasSchool/CLAUDE.md
- ~/Business/PropertyInvestmentGroup/CLAUDE.md

Summarise the portfolio and ask what Jamie would like to discuss across his businesses.

Consider cross-business synergies, time allocation, strategic priorities, and overall portfolio health.
```

---

## MCP Servers (Model Context Protocol)

MCP servers extend Claude Code's capabilities with external tools.

### Currently Configured Servers

| Server | Command | Purpose |
|--------|---------|---------|
| **filesystem** | `npx -y @modelcontextprotocol/server-filesystem /Users/jamiesmyth` | File system access |
| **sequential-thinking** | `npx -y @modelcontextprotocol/server-sequential-thinking` | Step-by-step reasoning |
| **github** | `npx -y @modelcontextprotocol/server-github` | GitHub integration |
| **context7** | `npx -y @upstash/context7-mcp` | Documentation lookup |

### Adding MCP Servers

```bash
# Add a new MCP server
claude mcp add <server-name> -- <command>

# Example: Add GitHub server
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# List configured servers
claude mcp list

# Remove a server
claude mcp remove <server-name>
```

### Useful MCP Servers for Property Tech

```bash
# GitHub for code management
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# Context7 for documentation lookup
claude mcp add context7 -- npx -y @upstash/context7-mcp

# Sequential thinking for complex analysis
claude mcp add sequential-thinking -- npx -y @modelcontextprotocol/server-sequential-thinking

# Filesystem for file access
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem ~/
```

---

## Hooks - Automated Actions

Hooks run automatically in response to Claude Code events.

### Current Hooks Configuration

**Auto-format TypeScript/JavaScript:**
```json
{
  "matcher": "Edit|Write",
  "hooks": [{
    "type": "command",
    "command": "if [[ \"$CLAUDE_FILE_PATH\" == *.ts ]] || [[ \"$CLAUDE_FILE_PATH\" == *.tsx ]]; then npx prettier --write \"$CLAUDE_FILE_PATH\" 2>/dev/null || true; fi"
  }]
}
```

**Auto-format Python:**
```json
{
  "matcher": "Edit|Write",
  "hooks": [{
    "type": "command",
    "command": "if [[ \"$CLAUDE_FILE_PATH\" == *.py ]]; then black \"$CLAUDE_FILE_PATH\" 2>/dev/null || true; fi"
  }]
}
```

### Hook Events

| Event | Trigger |
|-------|---------|
| `PreToolUse` | Before a tool runs |
| `PostToolUse` | After a tool completes |
| `Notification` | When Claude sends a notification |
| `Stop` | When Claude stops execution |

### Adding Custom Hooks

Add to `~/.claude/settings.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Bash",
        "hooks": [{
          "type": "command",
          "command": "echo 'Bash command completed' >> ~/.claude/audit.log"
        }]
      }
    ]
  }
}
```

---

## Custom Agents

Agents are specialized sub-agents that can be invoked for specific tasks.

### Creating an Agent

Create in `~/.claude/agents/<agent-name>.md`:

```markdown
---
name: property-analyst
description: "Use this agent when analysing commercial property data, rent reviews, or market comparables."
model: sonnet
color: blue
---

You are a Commercial Property Analyst with expertise in UK commercial real estate...

## Your Role
[Define the agent's responsibilities]

## Working Method
[Define how the agent should approach tasks]

## Standards & Best Practice
[Reference relevant standards - RICS, regulations, etc.]

## Communication Style
[Define output format and tone]
```

### Example: Residential Architect Agent

See `~/.claude/agents/residential-architect.md` for a complete example of a specialized agent for architectural review and planning guidance.

---

## Persistent Memory

Claude Code maintains memory across sessions in `~/.claude/projects/<project-hash>/memory/MEMORY.md`.

### Memory Best Practices

1. **Keep it concise** - Lines after 200 are truncated
2. **Organize semantically** - By topic, not chronologically
3. **Link to files** - Reference full docs rather than duplicating
4. **Update actively** - Remove outdated information

### Example Memory Structure

```markdown
# Memory

## Active Projects
- Project A: [status, key files, next steps]
- Project B: [status, key files, next steps]

## Key Decisions
- [Date]: Decision made, rationale, outcome

## Technical Patterns
- Pattern A: [when to use, how to implement]
```

---

## Recommended Plugins

Available from the official plugin marketplace:

| Plugin | Purpose |
|--------|---------|
| `commit-commands` | Enhanced git workflow |
| `code-review` | Structured code reviews |
| `feature-dev` | Feature development workflow |
| `pr-review-toolkit` | PR review automation |
| `plugin-dev` | Create your own plugins |

### Installing Plugins

```bash
# Plugins auto-install from marketplace
# Or manually clone to ~/.claude/plugins/
```

---

## Permissions Configuration

### settings.local.json (Machine-Specific)

This file contains local permission overrides and is not committed to version control.

```json
{
  "permissions": {
    "allow": [
      "WebSearch",
      "WebFetch(domain:www.redbookassist.uk)",
      "Bash(git add:*)",
      "Bash(git commit:*)",
      "Bash(pnpm build:*)",
      "Bash(npm run build:*)",
      "mcp__github__search_repositories",
      "mcp__context7__resolve-library-id",
      "mcp__sequential-thinking__sequentialthinking",
      "mcp__filesystem__list_directory"
    ]
  }
}
```

---

## Quick Reference

### Common Commands

| Command | Purpose |
|---------|---------|
| `/commit` | Create git commit |
| `/pr` | Create pull request |
| `/review` | Review code changes |
| `/test` | Run test suite |
| `/explain` | Explain codebase |
| `/prime` | Load project context |
| `/business` | Business analysis |
| `/hillway` | Switch to Hillway context |

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Ctrl+C` | Cancel current operation |
| `Ctrl+D` | Exit Claude Code |
| `Tab` | Autocomplete |
| `Up/Down` | Navigate history |

### Environment Variables

```bash
# Required
ANTHROPIC_API_KEY=sk-ant-...

# Optional
CLAUDE_MODEL=claude-opus-4-5-20251101
GITHUB_TOKEN=ghp_...
```

---

## Hillway Digital Setup Checklist

For new team members:

- [ ] Install Node.js 18+
- [ ] Install Claude Code: `npm install -g @anthropic-ai/claude-code`
- [ ] Copy `~/.claude/CLAUDE.md` from this guide
- [ ] Copy `~/.claude/settings.json` from this guide
- [ ] Add custom commands to `~/.claude/commands/`
- [ ] Configure MCP servers: `claude mcp list`
- [ ] Set up API key: `export ANTHROPIC_API_KEY=...`
- [ ] Test: `claude` and try `/prime`

---

## Support

- **Claude Code Issues:** https://github.com/anthropics/claude-code/issues
- **Hillway Internal:** Contact Digital Department
- **Documentation:** https://docs.anthropic.com/claude-code
