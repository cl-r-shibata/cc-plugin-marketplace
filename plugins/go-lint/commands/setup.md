---
description: Install golangci-lint and create .golangci.yml configuration with best practice settings
argument-hint: "[path]"
allowed-tools: ["Bash", "Write", "Read", "Glob"]
---

# Setup Go Linting Environment

Set up golangci-lint and create a best practice configuration file for Go linting.

## Instructions

Follow these steps to set up the Go linting environment:

### 1. Check golangci-lint Installation

First, check if golangci-lint is already installed:

```bash
golangci-lint version
```

**If not installed**, provide installation instructions based on the operating system:

**For macOS (Homebrew):**
```bash
brew install golangci-lint
```

**For Linux/macOS (using go install):**
```bash
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
```

**For Windows (using go install):**
```bash
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
```

After installation, verify with `golangci-lint version`.

### 2. Determine Target Directory

- If the user provided a path argument, use that directory
- Otherwise, use the current working directory
- Verify the directory exists using appropriate tools

### 3. Check for Existing Configuration

Check if `.golangci.yml` already exists in the target directory:

- If it exists, inform the user and ask if they want to overwrite it
- If the user declines, exit gracefully
- If it doesn't exist or user agrees to overwrite, proceed to create the file

### 4. Create .golangci.yml Configuration

Create a `.golangci.yml` file with the following best practice configuration:

```yaml
run:
  timeout: 5m
  tests: true
  modules-download-mode: readonly

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
    - exportloopref
    - gocyclo
    - dupl

linters-settings:
  govet:
    check-shadowing: true
    enable-all: true

  gocyclo:
    min-complexity: 15

  dupl:
    threshold: 100

  errcheck:
    check-type-assertions: true
    check-blank: true

  revive:
    rules:
      - name: exported
        severity: warning
        disabled: false
      - name: var-naming
        severity: warning
        disabled: false
      - name: package-comments
        severity: warning
        disabled: false

  gosec:
    excludes:
      - G104  # Audit errors not checked - covered by errcheck

issues:
  exclude-use-default: false
  max-issues-per-linter: 0
  max-same-issues: 0

  # Exclude some false positives
  exclude-rules:
    - path: _test\.go
      linters:
        - gocyclo
        - dupl
        - gosec

output:
  sort-results: true
```

### 5. Confirm Success

After creating the configuration:

1. Inform the user that `.golangci.yml` has been created successfully
2. Show the path to the configuration file
3. Suggest next steps:
   - Run `/go-lint:lint` to check existing code
   - Run `/go-lint:init-project` if starting a new project
   - Customize `.golangci.yml` based on project needs

## Tips

- The configuration enables essential linters while balancing strictness with practicality
- Tests are excluded from complexity checks (gocyclo) and duplication checks (dupl)
- You can customize the configuration after creation based on project-specific needs
- For existing projects with many issues, consider using `--new-from-rev` flag to lint only new code

## Example Usage

```
User: /go-lint:setup
Assistant: [Checks golangci-lint, creates .golangci.yml in current directory]

User: /go-lint:setup ./myproject
Assistant: [Checks golangci-lint, creates .golangci.yml in ./myproject]
```
