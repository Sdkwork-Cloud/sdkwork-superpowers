# MiMoCode Tool Mapping

Skills use Claude Code tool names. When you encounter these in a skill, use your platform equivalent:

| Skill references | MiMoCode equivalent |
|-----------------|---------------------|
| `Task` tool (dispatch subagent) | `@mention` syntax or automatic dispatch |
| Multiple `Task` calls (parallel) | Multiple `@mention` calls |
| `TodoWrite` (task tracking) | `todowrite` |
| `Skill` tool (invoke a skill) | MiMoCode's native `skill` tool |
| `Read`, `Write`, `Edit` (files) | Use your native file tools |
| `Bash` (run commands) | Use your native shell tools |
| `Grep` (search file content) | Use your native search tools |
| `Glob` (search files by name) | Use your native file tools |
| `WebFetch` | Use your native web tools |
| `WebSearch` | No equivalent — use `web_fetch` with a search engine URL |

## Subagent support

MiMoCode supports subagents natively via the `@` syntax. Use the built-in `@mention` system to dispatch any task.

When a skill says to dispatch a named agent type, use `@mention` with the full prompt from the skill's prompt template:

| Skill instruction | MiMoCode equivalent |
|-------------------|---------------------|
| `Task tool (superpowers:implementer)` | `@mention` with the filled `implementer-prompt.md` template |
| `Task tool (superpowers:spec-reviewer)` | `@mention` with the filled `spec-reviewer-prompt.md` template |
| `Task tool (superpowers:code-reviewer)` | `@mention` with the filled review prompt |
| `Task tool (superpowers:code-quality-reviewer)` | `@mention` with the filled `code-quality-reviewer-prompt.md` template |
| `Task tool (general-purpose)` with inline prompt | `@mention` with your inline prompt |

### Prompt filling

Skills provide prompt templates with placeholders like `{WHAT_WAS_IMPLEMENTED}` or `[FULL TEXT of task]`. Fill all placeholders and pass the complete prompt as the message to `@mention`. The prompt template itself contains the agent's role, review criteria, and expected output format.

## Environment Detection

Skills that create worktrees or finish branches should detect their environment with read-only git commands before proceeding:

```bash
GIT_DIR=$(cd "$(git rev-parse --git-dir)" 2>/dev/null && pwd -P)
GIT_COMMON=$(cd "$(git rev-parse --git-common-dir)" 2>/dev/null && pwd -P)
BRANCH=$(git branch --show-current)
```

- `GIT_DIR != GIT_COMMON` → already in a linked worktree (skip creation)
- `BRANCH` empty → detached HEAD (cannot branch/push/PR from sandbox)

See `using-git-worktrees` Step 0 and `finishing-a-development-branch` Step 1 for how each skill uses these signals.

## MiMoCode-Specific Notes

MiMoCode is derived from OpenCode and shares the same plugin API and hooks. Key differences:

| Aspect | OpenCode | MiMoCode |
|--------|----------|----------|
| Config directory | `~/.config/opencode/` | `~/.config/mimocode/` |
| Config file | `opencode.json` | `mimocode.jsonc` |
| Plugin directory | `.opencode/plugins/` | `.mimocode/plugins/` |
| Skills directory | `~/.config/opencode/skills/` | `~/.config/mimocode/skills/` |
| Binary command | `opencode` | `mimo` |
| Plugin install | `opencode plugin <module>` | `mimo plugin <module>` |
