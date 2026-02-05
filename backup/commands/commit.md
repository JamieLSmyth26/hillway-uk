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
