# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a Claude Code plugin marketplace repository. It hosts plugins that extend Claude Code functionality through slash commands, agents, skills, and hooks. Users install plugins from this marketplace using the `/plugin` command in Claude Code.

## Architecture

### Marketplace Structure

The repository follows the Claude Code marketplace convention:

- **`.claude-plugin/marketplace.json`**: The marketplace manifest that defines available plugins. Each plugin entry includes:
  - `name`: Plugin identifier
  - `version`: Semantic version
  - `description`: Brief description
  - `source`: Relative path to plugin directory (e.g., `./plugins/greeting`)
  - `author`: Author information with name and URL
  - `tags`: Array of searchable tags

- **`plugins/` directory**: Contains individual plugin directories. Each plugin is self-contained with:
  - `plugin.json`: Plugin manifest (name, version, description, author)
  - `commands/`: Slash command definitions as Markdown files with YAML frontmatter
  - `agents/`: (optional) Autonomous agent definitions
  - `skills/`: (optional) Specialized skill definitions
  - `hooks/`: (optional) Event-driven hook scripts
  - `README.md`: Plugin-specific documentation

### Plugin Installation Flow

Users install plugins via:
1. `/plugin marketplace add cl-r-shibata/cc-plugin-marketplace` - Adds this marketplace
2. `/plugin install greeting@cc-plugin-marketplace` - Installs specific plugin
3. `/plugin menu` - Browse available plugins

Alternative: Direct GitHub URL or NPX CLI installation.

### Command Structure

Slash commands in `commands/*.md` use YAML frontmatter:
- `description`: Command description shown in help
- `argument-hint`: Usage hint for arguments
- `allowed-tools`: Array of tools the command can use (empty array means Claude determines tools)

The Markdown body contains the prompt that executes when the command runs.

## Adding New Plugins

1. Create plugin directory under `plugins/`
2. Add `plugin.json` with metadata
3. Create `commands/` directory with command definitions
4. Add plugin entry to `.claude-plugin/marketplace.json`:
   - Set correct `source` path relative to repository root
   - Use `./plugins/plugin-name` format
   - Include author info matching marketplace owner
   - Add descriptive tags for discoverability

5. Update main `README.md` with plugin information

## Key Files

- `.claude-plugin/marketplace.json`: Central marketplace registry
- `plugins/*/plugin.json`: Individual plugin manifests
- `plugins/*/commands/*.md`: Slash command implementations
