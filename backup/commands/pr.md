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
