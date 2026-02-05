# Global Claude Code Configuration

## About Me
- Name: Jamie Smyth
- Focus: Full-stack SaaS, AI/Agent projects, various web applications
- Languages: TypeScript/Node.js and Python (both equally)
- Businesses: Managing existing businesses (strategic focus)

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
