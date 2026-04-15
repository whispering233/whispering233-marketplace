# whispering233-marketplace

Marketplace repository scaffold for local plugins that can be consumed by both Codex and Claude Code.

## Layout

- `.agents/plugins/marketplace.json`: Codex marketplace index
- `.claude-plugin/marketplace.json`: Claude Code marketplace index
- `plugins/<plugin-name>/`: each plugin payload

## Included starter plugin

This repo starts with `plugins/example-plugin/` as a minimal example. Replace it or duplicate it when adding real plugins.

## Add a new plugin

1. Copy `plugins/example-plugin` to `plugins/<your-plugin-name>`.
2. Update `plugins/<your-plugin-name>/.codex-plugin/plugin.json`.
3. Add an entry for the plugin in both marketplace files.

