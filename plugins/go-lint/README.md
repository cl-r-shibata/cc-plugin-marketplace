# Go Lint Plugin

A Claude Code plugin for setting up and managing Go linter environments using golangci-lint.

## Features

- **Quick Setup**: Install and configure golangci-lint with best practice settings
- **Project Initialization**: Create Hello World Go projects with linter configuration
- **Lint Execution**: Run comprehensive linting checks on your Go code
- **Automatic Fixes**: Apply automatic fixes for common linting issues
- **AI-Powered Support**: Intelligent agent that suggests linting after code changes

## Prerequisites

- Go 1.16 or later installed
- Internet connection for downloading golangci-lint (if not already installed)

## Installation

### From Marketplace

```bash
# Add this marketplace (if not already added)
/plugin marketplace add cl-r-shibata/cc-plugin-marketplace

# Install the plugin
/plugin install go-lint@cc-plugin-marketplace
```

### From Local Directory

```bash
# Copy plugin to your project
cp -r plugins/go-lint /your/project/.claude-plugin/

# Or use --plugin-dir flag
cc --plugin-dir /path/to/plugins/go-lint
```

## Usage

### Commands

#### `/go-lint:setup`
Install golangci-lint and create `.golangci.yml` configuration file with best practice settings.

```bash
/go-lint:setup
```

#### `/go-lint:init-project [module-name]`
Initialize a new Go project with Hello World code and linter configuration.

```bash
/go-lint:init-project github.com/user/myproject
```

#### `/go-lint:lint [path]`
Run linting checks on your Go code. Defaults to checking all packages (`./...`).

```bash
/go-lint:lint
/go-lint:lint ./cmd/...
```

#### `/go-lint:fix [path]`
Automatically fix linting issues where possible.

```bash
/go-lint:fix
/go-lint:fix ./pkg/...
```

### Skill

The plugin includes a **go-linter-best-practices** skill that activates when you ask questions about:
- Go linting configuration
- golangci-lint setup and usage
- Common linting errors and their fixes
- Best practices for Go code quality

Example queries:
- "What's the recommended golangci-lint configuration?"
- "How do I fix this linting error?"
- "What linters should I enable for Go?"

### Agent

The **go-lint-checker** agent proactively suggests running lint checks after Claude modifies Go code, helping maintain code quality automatically.

## Example Workflow

1. Create a new Go project:
   ```bash
   /go-lint:init-project github.com/myuser/hello
   ```

2. Claude creates project structure with Hello World code

3. Run lint check:
   ```bash
   /go-lint:lint
   ```

4. Fix any issues:
   ```bash
   /go-lint:fix
   ```

5. Agent automatically suggests checks after future code changes

## Configuration

The plugin generates a `.golangci.yml` file with recommended linter settings including:
- Essential linters: govet, staticcheck, gosimple, unused
- Code quality linters: errcheck, ineffassign, gofmt, goimports
- Security linters: gosec
- Optimized for both correctness and readability

You can customize the generated `.golangci.yml` file according to your project needs.

## Troubleshooting

**golangci-lint not found**
- The plugin will guide you through installation
- Manual install: `go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest`

**Linting takes too long**
- Use path argument to lint specific packages: `/go-lint:lint ./cmd/...`
- Adjust `.golangci.yml` timeout settings

## License

MIT
