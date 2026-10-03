# tmux Cheatsheet

Default tmux keybindings and commands. Personal/custom bindings are in `personal-cheatsheet.md`.

## Prefix

Default prefix: `Ctrl+b`. Most keybindings are `Ctrl+b` → key.
Example: `Ctrl+b c` creates a new window.

---

## Sessions

| Action | Command / Key |
|---|---|
| Start tmux | `tmux` |
| Create named session | `tmux new-session -s name` (short: `tmux new -s name`) |
| List sessions | `tmux ls` or `Ctrl+b s` (inside tmux) |
| Attach to last session | `tmux attach` (short: `tmux a`) |
| Attach to specific session | `tmux attach -t name` |
| Detach (session keeps running) | `Ctrl+b d` |
| Rename current session | `Ctrl+b $` |
| Kill a session | `tmux kill-session -t name` |
| Kill current session | no default key; run `tmux kill-session` (no `-t`) from a shell inside it |

---

## Windows

A window is like a tab inside a session.

| Action | Key |
|---|---|
| Create | `Ctrl+b c` |
| Next | `Ctrl+b n` |
| Previous | `Ctrl+b p` |
| Last/previously active | `Ctrl+b l` |
| Select by number | `Ctrl+b 0` … `Ctrl+b 9` |
| Choose interactively | `Ctrl+b w` |
| Rename | `Ctrl+b ,` |
| Kill | `Ctrl+b &` |
| Move left | `Ctrl+b <` |
| Move right | `Ctrl+b >` |

---

## Panes

A pane is a split area inside a window.

**Split vertically** (`Ctrl+b %`)

```
┌──────────┬──────────┐
│          │          │
│  Pane    │  Pane    │
│          │          │
└──────────┴──────────┘
```

**Split horizontally** (`Ctrl+b "`)

```
┌─────────────────────┐
│       Pane          │
├─────────────────────┤
│       Pane          │
└─────────────────────┘
```

| Action | Key |
|---|---|
| Move between panes | `Ctrl+b ←` `→` `↑` `↓` |
| Next pane | `Ctrl+b o` |
| Previous pane | `Ctrl+b ;` |
| Show pane numbers | `Ctrl+b q` |
| Kill current pane | `Ctrl+b x` |
| Toggle zoom (fill window; press again to restore) | `Ctrl+b z` |
| Swap with previous (left/up) | `Ctrl+b {` |
| Swap with next (right/down) | `Ctrl+b }` |

### Resizing

| Action | Key |
|---|---|
| Resize by a small amount | `Ctrl+b Ctrl+Arrow` (hold `Ctrl`, press arrows) |
| Resize by a larger amount | `Ctrl+b Alt+Arrow` |

---

## Layouts

| Layout | Key |
|---|---|
| Even horizontal | `Ctrl+b Alt+1` |
| Even vertical | `Ctrl+b Alt+2` |
| Main horizontal | `Ctrl+b Alt+3` |
| Main vertical | `Ctrl+b Alt+4` |
| Tiled | `Ctrl+b Alt+5` |
| Cycle layouts | `Ctrl+b Space` |

---

## Copy Mode

Enter: `Ctrl+b [`

| Action | Key |
|---|---|
| Move up/down | Arrow keys |
| Page up / down | `PageUp` / `PageDown` |
| Search forward | `/` |
| Search backward | `?` |
| Quit | `q` |

With the default emacs-style copy mode, selection/copying depends on the configured mode.

---

## Mouse

Off by default. Enable in `~/.tmux.conf`:

```
set -g mouse on
```

Then you can select panes and windows, resize panes, scroll, and select text.

---

## Configuration

Default file: `~/.tmux.conf`

Reload from shell:

```
tmux source-file ~/.tmux.conf
```

Or inside tmux: `Ctrl+b :` then `source-file ~/.tmux.conf`

---

## Command Mode

Open: `Ctrl+b :`, then enter commands such as `list-sessions`, `list-windows`, `list-panes`.

---

## Information

| Action | Command |
|---|---|
| Version | `tmux -V` |
| List sessions | `tmux ls` |
| List windows | `tmux list-windows` |
| List panes | `tmux list-panes` |
| Show options | `tmux show-options -g` |
| Show keybindings | `tmux list-keys` |

---

## Common Commands

```bash
# Start tmux
tmux

# Create named session
tmux new -s name

# List sessions
tmux ls

# Attach
tmux attach

# Attach to specific session
tmux attach -t name

# Kill session
tmux kill-session -t name

# Kill all tmux sessions
tmux kill-server

# Show version
tmux -V

# Reload configuration
tmux source-file ~/.tmux.conf
```

---

## Default Keybinding Reference

| Action | Default key |
|---|---|
| Prefix | `Ctrl+b` |
| List sessions | `Ctrl+b s` |
| Detach | `Ctrl+b d` |
| Rename session | `Ctrl+b $` |
| New window | `Ctrl+b c` |
| Next window | `Ctrl+b n` |
| Previous window | `Ctrl+b p` |
| Last window | `Ctrl+b l` |
| Select window | `Ctrl+b 0–9` |
| Choose window | `Ctrl+b w` |
| Rename window | `Ctrl+b ,` |
| Kill window | `Ctrl+b &` |
| Vertical split | `Ctrl+b %` |
| Horizontal split | `Ctrl+b "` |
| Move pane | `Ctrl+b Arrow` |
| Next pane | `Ctrl+b o` |
| Previous pane | `Ctrl+b ;` |
| Show pane numbers | `Ctrl+b q` |
| Kill pane | `Ctrl+b x` |
| Zoom pane | `Ctrl+b z` |
| Swap pane left/up | `Ctrl+b {` |
| Swap pane right/down | `Ctrl+b }` |
| Resize pane | `Ctrl+b Ctrl+Arrow` |
| Cycle layout | `Ctrl+b Space` |
| Copy mode | `Ctrl+b [` |
| Command mode | `Ctrl+b :` |
| List keybindings | `tmux list-keys` |

New session has no key; use `tmux new -s name`.

---

## Quick Mental Reference

```
SESSION
│
├── WINDOW 1
│   ├── PANE
│   └── PANE
│
├── WINDOW 2
│   └── PANE
│
└── WINDOW 3
    ├── PANE
    └── PANE
```

```
Session → workspace
Window  → tab
Pane    → split
```

### Most important keys

```
Ctrl+b c      new window
Ctrl+b n      next window
Ctrl+b p      previous window
Ctrl+b %      vertical split
Ctrl+b "      horizontal split
Ctrl+b Arrow  move pane
Ctrl+b z      zoom pane
Ctrl+b d      detach
Ctrl+b s      choose session
Ctrl+b ,      rename window
Ctrl+b $      rename session
Ctrl+b x      kill pane
```
