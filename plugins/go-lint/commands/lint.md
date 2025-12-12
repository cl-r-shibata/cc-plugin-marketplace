---
description: Run golangci-lint to check Go code for issues
argument-hint: "[path]"
allowed-tools: ["Bash", "Read", "Glob"]
---

# Run Go Linting

Execute golangci-lint to check Go code for potential issues, style violations, and bugs.

## Instructions

Follow these steps to run linting checks:

### 1. Check golangci-lint Installation

Verify that golangci-lint is installed:

```bash
which golangci-lint
```

**If not installed**, provide installation instructions:

```
golangci-lint is not installed. Please install it first:

For macOS (Homebrew):
  brew install golangci-lint

For Linux/macOS/Windows (using go install):
  go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest

Then ensure $GOPATH/bin is in your PATH.

Or use /go-lint:setup to set up the complete environment.
```

Exit if not installed.

### 2. Determine Target Path

- If the user provided a path argument, use that (e.g., `./cmd/...`, `./pkg/server`)
- Otherwise, use `./...` to check all packages in the current directory and subdirectories
- Validate the path exists if it's a specific directory

### 3. Check for Configuration

Check if `.golangci.yml` exists in the current directory or parent directories:

- If found, mention that using existing configuration
- If not found, inform the user that default linters will be used
- Suggest running `/go-lint:setup` to create a best-practice configuration

### 4. Run golangci-lint

Execute the linting command:

```bash
golangci-lint run <path>
```

For example:
- `golangci-lint run ./...` - Check all packages
- `golangci-lint run ./cmd/...` - Check only cmd directory
- `golangci-lint run main.go` - Check specific file

Set a timeout to prevent hanging on large codebases:
```bash
golangci-lint run --timeout=5m <path>
```

### 5. Parse and Present Results

**If no issues found:**
```
✓ No linting issues found!

Your Go code passes all enabled linter checks.
```

**If issues are found:**

Parse the output and present it in a clear, organized format:

1. **Summary** - Total number of issues found
2. **Issues by severity** - Group by linter/category if possible
3. **Detailed list** - Show file, line, column, message, and linter name

Example format:
```
Linting Results
===============

Found 5 issues:

❌ main.go:15:2
   ineffectual assignment to err (ineffassign)

   err := doSomething()
   err = doSomethingElse()  // This overwrites err without checking it

   Fix: Check the first error before reassigning

❌ handler.go:42:9
   Error return value of `file.Close` is not checked (errcheck)

   defer file.Close()

   Fix: Handle the error properly:
   defer func() {
       if err := file.Close(); err != nil {
           log.Printf("failed to close: %v", err)
       }
   }()

... (remaining issues)

To automatically fix some issues, run: /go-lint:fix
```

### 6. Provide Actionable Guidance

Based on the results:

**For common error patterns, provide specific fixes:**

- **Unused variables**: "Remove the variable or use it"
- **Unchecked errors**: "Add error checking with if err != nil"
- **Formatting issues**: "Run `go fmt` or `/go-lint:fix`"
- **Missing comments**: "Add godoc comments to exported functions"

**For security issues (gosec):**
- Highlight with higher priority
- Explain the security implication
- Provide secure alternative

**For high complexity (gocyclo):**
- Suggest breaking down the function
- Recommend extracting helper functions

### 7. Suggest Next Steps

After presenting results:

```
Next steps:
• Run /go-lint:fix to automatically fix some issues
• Review and manually fix remaining issues
• Check individual linter documentation for details
• Use /go-lint:lint ./path to check specific directories
• Ask me about specific linting errors for help
```

## Error Handling

**If golangci-lint fails to run:**
- Check if the directory contains Go files
- Verify go.mod exists for module-based projects
- Check for syntax errors in Go files
- Suggest running `go build` first to check for compilation errors

**If timeout occurs:**
- Suggest using a specific path instead of `./...`
- Recommend increasing timeout in `.golangci.yml`
- Consider disabling slow linters like `dupl`

## Tips

- For large codebases, lint specific packages to get faster results
- Use `--new-from-rev` to check only changed files: `golangci-lint run --new-from-rev=HEAD~1`
- Check output of `golangci-lint linters` to see all available linters
- Some issues can be auto-fixed with `/go-lint:fix`, others require manual intervention

## Example Usage

```
User: /go-lint:lint
Assistant: [Runs golangci-lint run ./..., presents results with fixes]

User: /go-lint:lint ./cmd/server
Assistant: [Runs golangci-lint on cmd/server directory only]

User: /go-lint:lint main.go
Assistant: [Runs golangci-lint on main.go file]
```
