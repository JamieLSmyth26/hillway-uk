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
