# CC Plugin Marketplace

A marketplace for Claude Code plugins.

## Available Plugins

### Greeting Plugin
Simple greeting plugin with Japanese support.
- **Version**: 0.1.0
- **Command**: `/greeting:hello`
- **Description**: Say hello in Japanese (こんにちは - Konnichiwa)

## Installation

### Install from this repository

```bash
# Clone this repository
git clone <repository-url>

# Install a specific plugin
cc plugins install ./cc-plugin-marketplace/plugins/greeting
```

### Or use the marketplace directly

```bash
cc plugins install <marketplace-url>
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
