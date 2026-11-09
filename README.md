# agent-plugins

Plugins for AI agents, written once and shared across tools.

## Plugins

| Plugin | What it does |
|---|---|
| `terse` | Output style, applied automatically, that keeps everything the agent writes minimal. |
| `git` | `/git:commit`: splits the working tree into separate Conventional Commits, shows the plan, commits after confirmation. |

## Install

Claude Code:

```
claude plugin marketplace add TomasAntunez/agent-plugins
claude plugin install terse@chuli
claude plugin install git@chuli
```

## Develop

Installed plugins come from the remote, so a change reaches them once it is pushed:

```
claude plugin validate .
git push
```

With auto-update on for the marketplace (`/plugin` → Marketplaces → `chuli` → Enable auto-update), Claude Code fetches new commits in the background after a session starts; run `/reload-plugins` or start a new session to load them. Plugins carry no `version`, so every commit counts as an update.

To update right away: `claude plugin update terse@chuli`.

## Layout

```
.claude-plugin/marketplace.json   # catalog, one entry per plugin
plugins/<name>/
  .claude-plugin/plugin.json      # Claude Code manifest
  skills/<skill>/SKILL.md         # shared across tools
  output-styles/                  # Claude Code only
```
