# CC Plugin Marketplace

A marketplace for Claude Code plugins.

## Available Plugins

### Greeting Plugin
Simple greeting plugin with Japanese support.
- **Version**: 0.1.0
- **Command**: `/greeting:hello`
- **Description**: Say hello in Japanese (こんにちは - Konnichiwa)

## Installation

### From Claude Code

Open Claude Code and run these commands:

1. **Add this marketplace:**
   ```
   /plugin marketplace add cl-r-shibata/cc-plugin-marketplace
   ```

2. **Install a plugin:**
   ```
   /plugin install greeting@cc-plugin-marketplace
   ```

   Or browse all available plugins:
   ```
   /plugin menu
   ```

### Alternative: NPX CLI (from terminal)

If you prefer terminal installation:
```bash
npx claude-plugins install @cl-r-shibata/cc-plugin-marketplace/greeting
```

### Alternative: Direct GitHub URL

```bash
# From within Claude Code
/plugin install https://github.com/cl-r-shibata/cc-plugin-marketplace/plugins/greeting
```

## Plugin Structure

```
cc-plugin-marketplace/
├── marketplace.json          # Marketplace configuration
├── plugins/                  # Plugin directory
│   └── greeting/            # Greeting plugin
│       ├── plugin.json      # Plugin manifest
│       ├── commands/        # Slash commands
│       │   └── hello.md     # Hello command
│       └── README.md        # Plugin documentation
└── README.md                # This file
```

## Usage

After installation, use plugins with their commands:

```
/greeting:hello
```

## Contributing

To add a plugin to this marketplace:

1. Create your plugin in the `plugins/` directory
2. Add an entry to `marketplace.json`
3. Submit a pull request

## License

See individual plugin directories for licensing information.
