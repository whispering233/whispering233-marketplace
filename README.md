# whispering233-marketplace

Marketplace repository scaffold for local plugins that can be consumed by both Codex and Claude Code.

## Layout

- `.agents/plugins/marketplace.json`: Codex marketplace index
- `.claude-plugin/marketplace.json`: Claude Code marketplace index
- `plugins/<plugin-name>/`: each plugin payload

## Add a new plugin

1. Create `plugins/<your-plugin-name>/`.
2. Add `plugins/<your-plugin-name>/.codex-plugin/plugin.json`.
3. Add an entry for the plugin in both marketplace files.
