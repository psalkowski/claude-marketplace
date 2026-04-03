# Plugin Release Checklist

When updating a plugin, bump the version in **both** files:

1. `.claude-plugin/marketplace.json` — the `version` field in the plugin's entry (this is what `/plugin` checks to determine the latest available version)
2. `plugins/<plugin-name>/.claude-plugin/plugin.json` — the `version` field (this is the installed plugin's self-reported version)

These two versions must always match. If the marketplace version is lower than the plugin version, users won't be prompted to upgrade.
