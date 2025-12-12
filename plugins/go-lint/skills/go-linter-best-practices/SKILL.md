---
name: go-linter-best-practices
description: Provides expertise in Go linting with golangci-lint, including configuration best practices, common linting errors and their fixes, and recommended linter settings for Go projects. Activated when users ask about "Go linting", "golangci-lint configuration", "Go code quality", "linting errors in Go", or need help with ".golangci.yml" setup.
---

# Go Linter Best Practices

Apply golangci-lint configuration best practices and help users resolve common linting issues in Go projects.

## Overview

golangci-lint is the standard meta-linter for Go that runs multiple linters in parallel with unified configuration. Use it to maintain code quality, catch bugs early, and enforce consistent style across Go projects.

## Recommended Configuration

### Essential Linters to Enable

Configure `.golangci.yml` with these core linters:

**Static Analysis & Bug Detection:**
- `govet` - Official Go tool for suspicious constructs
- `staticcheck` - Comprehensive static analysis (highly recommended)
- `gosimple` - Suggests code simplifications
- `unused` - Detects unused constants, variables, functions
- `errcheck` - Ensures error returns are checked
- `ineffassign` - Detects ineffectual assignments

**Code Style & Formatting:**
- `gofmt` - Enforces standard Go formatting
- `goimports` - Manages import statements automatically
- `revive` - Fast, configurable linter for style checks
- `misspell` - Finds commonly misspelled words

**Security:**
- `gosec` - Inspects code for security vulnerabilities

**Complexity & Maintainability:**
- `gocyclo` - Detects high cyclomatic complexity
- `dupl` - Finds code duplication

### Sample .golangci.yml

```yaml
run:
  timeout: 5m
  tests: true

linters:
  enable:
    - govet
    - staticcheck
    - gosimple
    - unused
    - errcheck
    - ineffassign
    - gofmt
    - goimports
    - revive
    - misspell
    - gosec

linters-settings:
  govet:
    check-shadowing: true
  gocyclo:
    min-complexity: 15
  dupl:
    threshold: 100
  errcheck:
    check-type-assertions: true
    check-blank: true

issues:
  exclude-use-default: false
  max-issues-per-linter: 0
  max-same-issues: 0
```

### Configuration Principles

1. **Start strict, relax as needed** - Enable comprehensive checks initially, then disable specific rules if they cause false positives for your use case

2. **Project-specific adjustments** - Customize based on project type:
   - CLI tools: Enable `gocyclo`, focus on error handling
   - Libraries: Enable `godot`, `godox` for documentation
   - Web services: Enable `gosec`, `bodyclose` for security

3. **Team consensus** - Agree on style preferences (e.g., line length, naming conventions) before enforcing

## Common Linting Errors and Fixes

### Error: "ineffectual assignment"

**Issue:** Variable assigned but never used
```go
result := calculate() // ineffectual assignment
result = calculateAgain()
```

**Fix:** Remove the unused assignment
```go
result := calculateAgain()
```

### Error: "Error return value not checked"

**Issue:** Ignoring error returns from functions
```go
file.Close() // error not checked
```

**Fix:** Always check errors
```go
if err := file.Close(); err != nil {
    log.Printf("failed to close file: %v", err)
}
```

Or use defer with proper error handling:
```go
defer func() {
    if err := file.Close(); err != nil {
        log.Printf("failed to close file: %v", err)
    }
}()
```

### Error: "variable declared but not used"

**Issue:** Unused variable declarations
```go
func example() {
    x := 10 // declared but not used
    return
}
```

**Fix:** Remove unused variables or use blank identifier if needed
```go
func example() {
    return
}
```

### Error: "should have comment or be unexported"

**Issue:** Exported functions/types lack documentation
```go
func ProcessData() {} // missing comment
```

**Fix:** Add godoc comment
```go
// ProcessData processes the input data and returns the result.
func ProcessData() {}
```

### Error: "if block ends with a return statement, so drop this else"

**Issue:** Unnecessary else after return
```go
if condition {
    return true
} else {
    return false
}
```

**Fix:** Remove else clause
```go
if condition {
    return true
}
return false
```

### Error: "this value of err is never used"

**Issue:** Error variable reassigned without checking previous value
```go
err := firstOperation()
err = secondOperation() // previous err never checked
```

**Fix:** Check errors sequentially
```go
if err := firstOperation(); err != nil {
    return err
}
if err := secondOperation(); err != nil {
    return err
}
```

## Best Practices for Using golangci-lint

### 1. Run Locally Before Committing

Integrate linting into your development workflow:
```bash
golangci-lint run
```

Use `--fix` flag for auto-fixable issues:
```bash
golangci-lint run --fix
```

### 2. CI/CD Integration

Add to GitHub Actions, GitLab CI, or other CI systems:
```yaml
- name: golangci-lint
  uses: golangci/golangci-lint-action@v3
  with:
    version: latest
```

### 3. Incremental Adoption

For existing codebases with many issues:
- Use `--new-from-rev` to check only new code
- Use `issues.exclude-rules` in config to temporarily ignore certain files
- Fix issues incrementally by category

### 4. Editor Integration

Configure your editor/IDE to run golangci-lint on save:
- VS Code: Use Go extension with "go.lintTool": "golangci-lint"
- GoLand: Built-in support in Settings → Tools → File Watchers
- Vim/Neovim: Use ALE or nvim-lint plugins

## Interpreting Lint Output

golangci-lint output format:
```
path/to/file.go:42:10: ineffectual assignment to result (ineffassign)
```

Components:
- **File path**: Location of the issue
- **Line:Column**: Exact position in code
- **Message**: Description of the problem
- **Linter**: Which linter detected it (in parentheses)

Prioritize fixes:
1. **Security issues** (gosec) - Fix immediately
2. **Bugs** (govet, staticcheck) - High priority
3. **Error handling** (errcheck) - High priority
4. **Code quality** (gosimple, unused) - Medium priority
5. **Style** (gofmt, revive) - Can be auto-fixed

## Performance Optimization

If linting is slow:

1. **Limit scope**: Lint specific packages
   ```bash
   golangci-lint run ./cmd/...
   ```

2. **Adjust timeout**: Increase in `.golangci.yml`
   ```yaml
   run:
     timeout: 10m
   ```

3. **Disable slow linters**: If not critical, disable linters like `dupl` or `gocyclo`

4. **Use cache**: golangci-lint caches results by default in `~/.cache/golangci-lint`

## Troubleshooting

**Issue: Too many false positives**
- Adjust linter settings in `.golangci.yml`
- Use `issues.exclude-rules` to ignore specific patterns
- Consider if the warning indicates actual technical debt

**Issue: Conflicting rules**
- Some linters may conflict (e.g., line length vs. readability)
- Disable less important linters or adjust thresholds
- Prioritize your team's preferences

**Issue: golangci-lint not found**
- Install: `go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest`
- Ensure `$GOPATH/bin` is in your `$PATH`
- Or use Docker: `docker run --rm -v $(pwd):/app -w /app golangci/golangci-lint:latest golangci-lint run`

## When to Run Linting

Integrate linting at multiple stages:

1. **During development** - Editor integration provides immediate feedback
2. **Before commits** - Git pre-commit hooks catch issues early
3. **In CI/CD** - Automated checks prevent broken code from merging
4. **After major refactors** - Ensure code quality after large changes

Use appropriate strictness at each stage - stricter in CI than local development to allow experimentation.

## Summary

- Use golangci-lint as the standard Go linting tool
- Start with recommended linter set and customize for project needs
- Always check error returns and handle them appropriately
- Integrate linting into development workflow, editor, and CI/CD
- Fix security and bug-related issues first, style issues later
- Document exported APIs with godoc comments
- Use `--fix` flag for automatic corrections where possible

For detailed linter descriptions and configuration options, refer to [golangci-lint documentation](https://golangci-lint.run/).
