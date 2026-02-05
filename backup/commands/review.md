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
