# dotfiles

Personal shell config and utility scripts.

## Layout

- `.zshrc` — zsh shell config (aliases, functions, PATH, tab completions). Run
  `help` for a live, auto-generated list of every command defined here.
- `bin/` — utility scripts (added to PATH)
  - `sleep-manager` — manage macOS sleep behavior (`status`, `disable`, `enable`)
- `claude/` — Claude Code config
  - `settings.json` — symlinked to `~/.claude/settings.json`. Carries `enabledPlugins`,
    `extraKnownMarketplaces`, and `permissions`, so plugins reproduce on a new machine
    (Claude re-clones the marketplaces and reinstalls whatever's enabled on first run).

The files in this repo are the source of truth. `~/.zshrc`, `~/bin/<script>`, and
`~/.claude/settings.json` are symlinks back into this repo, so editing either side edits both.

## The `help` command

`help` prints every custom command. The list is **generated at call time**, not
stored: it reads the leading `# name <args> — description` comment above each
`.zshrc` function and the header line of each `bin/` script. So a new command
shows up automatically — just give it that one-line comment in the same format.

`help` renders with the prettiest tool it finds, falling back so it never breaks:

1. [`gum`](https://github.com/charmbracelet/gum) — `brew install gum` (nicest)
2. [`glow`](https://github.com/charmbracelet/glow) — `brew install glow`
3. plain aligned columns — no dependency

Both renderers are optional; with neither installed you still get a clean list.

## Claude plugins & MCP

Plugins sync via `claude/settings.json` above — no extra step.

MCP is two separate things:

- **claude.ai connectors** (Gmail, Calendar, Drive, Canva, Hugging Face, …) are bound
  to your Anthropic account and sync automatically when you log in on a new machine.
  There is nothing to copy.
- **Local/stdio MCP servers** live inside `~/.claude.json`, which is a stateful file
  (OAuth tokens, project history, caches) and is **not** symlinked. We have none today.
  If you add one, sync it with a merge step rather than committing `~/.claude.json`.

## Install on a new machine

Clone the repo anywhere — `install.sh` is location-independent (it resolves its own
path), so the symlink *targets* follow wherever you checked it out:

```sh
git clone git@github.com:chrisjob1021/dotfiles.git path/to/dotfiles
path/to/dotfiles/install.sh
```

It creates the symlinks (backing up anything in the way to `*.bak`). If you ever move
the repo, just re-run `install.sh` from the new location to relink.
