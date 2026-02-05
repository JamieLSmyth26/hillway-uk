Fix GitHub issue #$ARGUMENTS

1. Fetch the issue details using `gh issue view $ARGUMENTS`
2. Analyze the issue and understand what needs to be fixed
3. Search the codebase to find relevant files
4. Plan the fix (use plan mode if complex)
5. Implement the fix
6. Run tests to verify
7. Create a commit with message: `fix: [description] (closes #$ARGUMENTS)`

If the issue is unclear, ask me for clarification before starting.
