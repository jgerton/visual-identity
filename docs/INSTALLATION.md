# Installation

## Prerequisites

- [Claude Code](https://claude.ai/claude-code) installed and working
- Git

## Try it on one project

Clone the repo and pass it as a plugin directory when you start Claude Code:

```bash
git clone https://github.com/jgerton/visual-identity.git
claude --plugin-dir ./visual-identity
```

This loads the plugin for the current session only.

## Install for every session

Clone into your Claude plugins directory:

```bash
git clone https://github.com/jgerton/visual-identity.git ~/.claude/plugins/visual-identity
```

Then add to your Claude Code settings (usually `~/.claude/settings.json`):

```json
{
  "plugins": ["~/.claude/plugins/visual-identity"]
}
```

## Verify installation

Start Claude Code:

```bash
claude
```

The plugin should load automatically. You can verify by asking Claude to list available skills. You should see `diagnosis` and `visual-direction` in the list.

## Updating

Pull the latest changes:

```bash
cd ~/.claude/plugins/visual-identity
git pull
```

Or if you cloned to a project directory, pull from there.
