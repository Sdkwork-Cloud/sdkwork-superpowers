# Installing Superpowers for MiMoCode

## Prerequisites

- [MiMoCode](https://github.com/mimo-ai/mimocode) installed (`npm install -g @mimo-ai/cli`)

## Installation

Add superpowers to the `plugin` array in your `mimocode.jsonc` (global or project-level):

```jsonc
{
  "plugin": ["superpowers@git+https://github.com/Sdkwork-Cloud/sdkwork-superpowers.git"]
}
```

Restart MiMoCode. The plugin installs through MiMoCode's plugin manager and
registers all skills.

Verify by asking: "Tell me about your superpowers"

MiMoCode uses its own plugin install. If you also use Claude Code, Codex, or
another harness, install Superpowers separately for each one.

## Migrating from the old symlink-based install

If you previously installed superpowers using `git clone` and symlinks, remove the old setup:

```bash
# Remove old symlinks
rm -f ~/.config/mimocode/plugins/superpowers.js
rm -rf ~/.config/mimocode/skills/superpowers

# Optionally remove the cloned repo
rm -rf ~/.config/mimocode/superpowers

# Remove skills.paths from mimocode.jsonc if you added one for superpowers
```

Then follow the installation steps above.

## Usage

Use MiMoCode's native `skill` tool:

```
use skill tool to list skills
use skill tool to load superpowers/brainstorming
```

## Updating

MiMoCode installs Superpowers through a git-backed package spec. Some MiMoCode
and Bun versions pin that resolved git dependency in a lockfile or cache, so a
restart may not pick up the newest Superpowers commit. If updates do not appear,
clear MiMoCode's package cache or reinstall the plugin.

To pin a specific version:

```jsonc
{
  "plugin": ["superpowers@git+https://github.com/Sdkwork-Cloud/sdkwork-superpowers.git#v5.0.3"]
}
```

## Troubleshooting

### Plugin not loading

1. Check logs: `mimo run --print-logs "hello" 2>&1 | grep -i superpowers`
2. Verify the plugin line in your `mimocode.jsonc`
3. Make sure you're running a recent version of MiMoCode

### Windows install issues

Some Windows MiMoCode builds have upstream installer issues with git-backed
plugin specs, including cache paths for `git+https` URLs and Bun not finding
`git.exe` even when it works in a normal terminal. If MiMoCode cannot install
the plugin, try installing with system npm and pointing MiMoCode at the local
package:

```powershell
npm install superpowers@git+https://github.com/Sdkwork-Cloud/sdkwork-superpowers.git --prefix "$HOME\.config\mimocode"
```

Then use the installed package path in `mimocode.jsonc`:

```jsonc
{
  "plugin": ["~/.config/mimocode/node_modules/superpowers"]
}
```

### Skills not found

1. Use `skill` tool to list what's discovered
2. Check that the plugin is loading (see above)

### Tool mapping

When skills reference Claude Code tools:
- `TodoWrite` → `todowrite`
- `Task` with subagents → `@mention` syntax
- `Skill` tool → MiMoCode's native `skill` tool
- File operations → your native tools

## Getting Help

- Report issues: https://github.com/Sdkwork-Cloud/sdkwork-superpowers/issues
- Full documentation: https://github.com/Sdkwork-Cloud/sdkwork-superpowers/blob/main/docs/README.mimocode.md
