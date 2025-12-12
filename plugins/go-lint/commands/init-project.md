---
description: Initialize a new Go project with Hello World code and linter configuration
argument-hint: "<module-name> [project-dir]"
allowed-tools: ["Bash", "Write", "Read"]
---

# Initialize Go Project with Linting

Create a new Go project with Hello World code, proper module initialization, and golangci-lint configuration.

## Instructions

Follow these steps to initialize a complete Go project:

### 1. Parse Arguments

Extract arguments from the user's command:

- **Required**: `module-name` - The Go module name (e.g., `github.com/user/project`, `example.com/myapp`)
- **Optional**: `project-dir` - Directory to create (defaults to last segment of module name)

If `module-name` is not provided, ask the user for it with an example:
```
Please provide a module name for your Go project.
Example: github.com/yourusername/projectname
```

### 2. Determine Project Directory

- If `project-dir` is provided, use it
- Otherwise, extract the last segment from module name (e.g., `github.com/user/hello` → `hello`)
- Verify the directory doesn't already exist
- If it exists, ask the user if they want to use a different name or overwrite

### 3. Create Project Structure

Create the project directory and navigate to it:

```bash
mkdir -p <project-dir>
cd <project-dir>
```

### 4. Initialize Go Module

Initialize the Go module:

```bash
go mod init <module-name>
```

Verify the `go.mod` file was created successfully.

### 5. Create Hello World main.go

Create a `main.go` file with properly formatted Hello World code:

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, World!")
}
```

### 6. Create .golangci.yml Configuration

Create a `.golangci.yml` file with best practice configuration (use the same configuration as in the `setup` command):

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
      - G104

issues:
  exclude-use-default: false
  max-issues-per-linter: 0
  max-same-issues: 0

  exclude-rules:
    - path: _test\.go
      linters:
        - gocyclo
        - dupl
        - gosec

output:
  sort-results: true
```

### 7. Create README.md

Create a basic README.md:

```markdown
# <Project Name>

A Go project initialized with golangci-lint configuration.

## Getting Started

### Run the application

```bash
go run main.go
```

### Run linting

```bash
golangci-lint run
```

### Build

```bash
go build
```

## Development

This project uses golangci-lint for code quality checks. The configuration is in `.golangci.yml`.
```

Replace `<Project Name>` with the last segment of the module name.

### 8. Verify and Test

Run the following commands to verify the setup:

1. **Run the program**:
   ```bash
   go run main.go
   ```
   Should output: `Hello, World!`

2. **Check with golangci-lint** (if installed):
   ```bash
   golangci-lint run
   ```
   Should show no errors (or inform user to install golangci-lint if not available)

### 9. Summary

Provide a summary to the user:

```
✓ Created project directory: <project-dir>
✓ Initialized Go module: <module-name>
✓ Created main.go with Hello World code
✓ Created .golangci.yml with best practice configuration
✓ Created README.md

Next steps:
1. cd <project-dir>
2. go run main.go  # Test the application
3. golangci-lint run  # Run linting (install golangci-lint if needed)
4. Start coding!

Use /go-lint:lint to check code quality anytime.
Use /go-lint:fix to automatically fix linting issues.
```

## Tips

- The generated code follows Go best practices and should pass all lint checks
- The project structure is minimal but production-ready
- You can add more files and packages as your project grows
- Consider adding a `.gitignore` file if using version control

## Example Usage

```
User: /go-lint:init-project github.com/myuser/hello
Assistant: [Creates ./hello directory with Go module, main.go, .golangci.yml, README.md]

User: /go-lint:init-project example.com/myapp myproject
Assistant: [Creates ./myproject directory with module name example.com/myapp]
```
