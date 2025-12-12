---
description: Automatically fix linting issues using golangci-lint --fix
argument-hint: "[path]"
allowed-tools: ["Bash", "Read", "Glob"]
---

# Automatically Fix Linting Issues

Run golangci-lint with the `--fix` flag to automatically correct fixable linting issues.

## Instructions

Follow these steps to automatically fix linting issues:

### 1. Check golangci-lint Installation

Verify that golangci-lint is installed:

```bash
which golangci-lint
```

**If not installed**, provide installation instructions and exit (same as lint command).

### 2. Determine Target Path

- If the user provided a path argument, use that (e.g., `./cmd/...`, `./pkg/server`)
- Otherwise, use `./...` to fix all packages in the current directory and subdirectories
- Validate the path exists if it's a specific directory

### 3. Show Warning About Auto-Fixing

Inform the user about what will happen:

```
⚠️  Auto-fix will modify your source files.

This will automatically fix issues that golangci-lint can safely correct, such as:
• Import formatting (goimports)
• Code formatting (gofmt)
• Unused code removal
• Simple style fixes

Note: Not all issues can be auto-fixed. Complex problems require manual intervention.

Proceeding with auto-fix on: <path>
```

### 4. Run golangci-lint with --fix

Execute the fix command:

```bash
golangci-lint run --fix --timeout=5m <path>
```

Capture both the output and any errors.

### 5. Parse and Present Results

**Check what was fixed:**

Compare the output before and after, or show the linter output which includes:
- Issues that were automatically fixed
- Issues that remain and need manual fixing

**Present in this format:**

```
Auto-Fix Results
================

✓ Fixed 8 issues automatically:
  • Formatted 3 files with gofmt
  • Fixed 4 import ordering issues (goimports)
  • Removed 1 unused variable (unused)

⚠️  3 issues require manual fixing:

❌ handler.go:42:9
   Error return value of `file.Close` is not checked (errcheck)

   This requires manual intervention - add error handling

❌ service.go:128:1
   Function complexity is too high (gocyclo)

   Consider breaking down this function into smaller parts

❌ model.go:55:1
   Exported function should have comment (revive)

   Add a godoc comment explaining what this function does
```

### 6. Show Diff Summary

If possible, provide a summary of what changed:

```
Modified files:
• main.go - formatted, fixed imports
• handler.go - formatted
• utils.go - removed unused variable
```

### 7. Verify Changes

After auto-fix completes:

1. **Run lint again** to show remaining issues:
   ```bash
   golangci-lint run <path>
   ```

2. **Present the current state**:
   ```
   After auto-fix:
   ✓ Fixed 8 issues automatically
   ⚠️  3 issues remain (require manual fixes)

   Progress: 73% of linting issues resolved
   ```

### 8. Provide Next Steps

Guide the user on what to do next:

```
Next steps:
• Review the changes made (use git diff if in a git repo)
• Manually fix remaining issues (I can help with these)
• Run /go-lint:lint to verify all issues are resolved
• Test your application to ensure functionality is unchanged
• Commit the changes
```

**If using git:**
Show the git diff to let user review changes:
```bash
git diff --stat
```

## What Can Be Auto-Fixed

Inform users about commonly auto-fixed issues:

**Automatically fixable:**
- Code formatting (gofmt, goimports)
- Import ordering and grouping
- Unused imports
- Simple unused variables (in some cases)
- Basic style violations (spacing, line breaks)

**Requires manual fixing:**
- Unchecked errors
- Logic issues
- Complex unused code
- Missing documentation
- Security vulnerabilities
- High cyclomatic complexity
- Type mismatches

## Error Handling

**If auto-fix fails:**
- Check for syntax errors in Go code
- Verify go.mod exists
- Ensure the directory contains Go files
- Try running `go build` first to identify compilation errors

**If some files couldn't be fixed:**
- Identify which files had issues
- Suggest checking those files manually
- Provide specific guidance based on error messages

## Safety Notes

Remind users:

```
Safety checklist after auto-fix:
□ Review changes with git diff (if using version control)
□ Run tests to ensure nothing broke
□ Verify application still builds: go build
□ Check that behavior is unchanged: go run main.go
```

## Tips

- Always commit your code before running auto-fix, so you can revert if needed
- Auto-fix is safe for formatting and import issues
- Review changes carefully before committing
- Some fixes might change code behavior (rare, but possible)
- Run `/go-lint:lint` after fixing to confirm all issues are resolved

## Example Usage

```
User: /go-lint:fix
Assistant: [Runs golangci-lint run --fix ./..., shows what was fixed and what remains]

User: /go-lint:fix ./cmd/server
Assistant: [Fixes issues in cmd/server directory only]

User: /go-lint:fix main.go
Assistant: [Fixes issues in main.go file only]
```
