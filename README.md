# Claude Marketplace

A personal collection of Claude Code plugins.

## Available Plugins

| Plugin | Description | Version |
| --- | --- | --- |
| autopilot | Fully autonomous issue-to-PR workflow with 23 specialized agents across 9 phases | 1.0.0 |

## Installation

```
/plugin marketplace add psalkowski/claude-marketplace
/plugin install autopilot
```

## Adding a Plugin

Each plugin lives in `plugins/<name>/` with the following structure:

```
plugins/<name>/
  .claude-plugin/
    plugin.json     # plugin manifest
  skills/           # skill definitions
  agents/           # agent definitions
```
