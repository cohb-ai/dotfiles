---
description: Push this Claude session into a detached background tmux session
---

Push the **current** Claude session into a detached tmux session so it keeps
running in the background and can be managed with the `dev`/`tgo`/`dev list`
tooling. This is the CLI `tpush` command, run on your behalf.

Do this:

1. Run `tpush` in the Bash tool. It auto-detects the current session via
   `$CLAUDE_CODE_SESSION_ID` and `$PWD`, spawns a detached `dev-<repo>-<slot>`
   tmux session running `claude -r <this-session-id>`, and prints the attach
   command.
2. Report the exact attach command it printed back to the user (e.g.
   `tgo ff 3` or `tmux attach -t dev-dotfiles-1`).
3. Remind the user that **this foreground Claude should now be exited** — the
   tmux copy owns the conversation from here. Do not keep working in this one.

Notes:
- If `tpush` says "Already inside tmux", this session is already backgrounded —
  just tell the user that and stop.
- To later pull it back to a normal terminal, that's `/tpop` (or the `tpop`
  shell command).
