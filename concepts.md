# tmux Concepts

## 1. What is tmux?

**tmux = terminal multiplexer.** It manages multiple terminal environments inside one terminal and keeps them organized and persistent.

```
Konsole
└── tmux
    └── Session
        ├── Window
        │   ├── Pane
        │   └── Pane
        └── Window
```

- **Konsole** → terminal emulator (provides the terminal window)
- **tmux** → terminal multiplexer (manages sessions, windows, panes inside it)

**Core purpose: three problems tmux solves**

1. **Organization**: Sessions → Windows → Panes
2. **Persistence**: detach → session continues
3. **Remote workflows**: SSH disconnects → tmux session remains → reconnect → attach

---

## 2. Architecture

### Server
When tmux starts, a **tmux server** manages all state. It owns sessions, windows, panes, and their running processes. This is why sessions survive detaching.

```
tmux server
├── Session
├── Session
└── Session
```

### Client
A **client** is the terminal interface connected to the server. Multiple clients can attach to the same server/session.

```
Konsole
   │
tmux client
   │
tmux server
   │
sessions
```

### Hierarchy

```
tmux server → Session → Window → Pane → Process
```

| Concept | Meaning |
|---|---|
| Session | workspace (separate projects/tasks) |
| Window | tab (different working contexts) |
| Pane | split inside a window |

```
Session: c-project
├── Window: editor
├── Window: terminal
└── Window: git
```

```
┌──────────────────┬──────────────────┐
│                  │                  │
│     Neovim       │      Bash        │
│                  │                  │
└──────────────────┴──────────────────┘
```

- Windows and panes are independent: each window has its own pane layout.
- Switching **windows** changes the whole workspace shown; switching **panes** changes the active area within the current window.

```
Window 1: Pane, Pane
Window 2: Pane
Window 3: Pane, Pane, Pane
```

---

## 3. Attach, Detach, Kill

| Action | Effect | Command |
|---|---|---|
| Attach | Connect to an existing session | `tmux attach` |
| Detach | Leave session; **it stays alive** | `Ctrl+b d` |
| Kill | **Destroy** session and its contents | `tmux kill-session -t name` |

```
Attached → Detach → Session continues → Attach again
```

> Detaching does **not** destroy a session. Killing does.

---

## 4. Persistence and SSH

tmux keeps the environment managed by its server alive after the client disconnects.

```
Laptop ── SSH ──▶ Remote server
                    └── tmux
                         ├── Neovim
                         ├── shell
                         └── process
```

When SSH disconnects:

```
SSH ✕
tmux ✓
process ✓
```

Reconnect and attach to continue.

**Limit:** tmux doesn't make every program immortal. A pane normally holds a shell or terminal program (`Pane → Bash → Neovim`); if that shell/process exits, the pane can close. tmux preserves the workspace and the processes it manages **while the server remains alive**.

---

## 5. Prefix Key

tmux uses a **prefix key** to distinguish tmux commands from normal terminal input. Default: `Ctrl+b`.

`Ctrl+b c` means: press `Ctrl+b` → release → press `c`.

---

## 6. Commands vs Keybindings

| Type | Use | Examples |
|---|---|---|
| Shell commands | Manage tmux from outside | `tmux ls`, `tmux new -s project`, `tmux attach -t project` |
| Keybindings | Work inside tmux | `Ctrl+b c`, `Ctrl+b d`, `Ctrl+b %` |

**Custom bindings** go in `~/.tmux.conf`:

```
bind h select-pane -L
```

Now `Ctrl+b h` moves left.

---

## 7. Targets

Many commands take a target (`-t`) for a session, window, or pane:

```
tmux kill-session -t project
```

Useful when managing many sessions/windows/panes.

**Pane identification:** panes are numbered internally as `session:window.pane`, e.g. `0:0.0`.

---

## 8. Naming

- **Sessions:** `tmux new -s c-project` (e.g. `c-project`, `lua-learning`, `linux-lab`)
- **Windows:** `1:editor  2:terminal  3:git`

Names make multiple sessions and the status bar easier to read.

---

## 9. Layouts, Zoom, Copy Mode

**Layouts**: tmux arranges panes automatically. Common layouts:
`even-horizontal`, `even-vertical`, `main-horizontal`, `main-vertical`, `tiled`

**Pane zoom**: temporarily make one pane fill the window. This changes the display only, not the underlying layout.

```
Normal:                     Zoom:
┌──────────┬──────────┐     ┌─────────────────────┐
│          │          │     │                     │
│  Pane    │  Pane    │     │      Pane           │
└──────────┴──────────┘     └─────────────────────┘
```

**Copy mode**: navigate and copy terminal history. Lets you scroll output, search history, select text, and copy text. Useful when normal terminal scrolling isn't enough.

---

## 10. Status Bar

Normally at the bottom. Shows session, windows, and extras like time:

```
c-project   1:editor  2:terminal  3:git       12:30
```

Customizable: colors, information, layout, keybindings.

---

## 11. Configuration

Config file: `~/.tmux.conf`. It can change:

- keybindings
- colors
- status bar
- pane borders
- mouse support
- window numbering
- terminal behavior
- default commands

Config can be **reloaded without restarting the server**.

**Mouse support** (optional): pane selection/resizing, window selection, scrolling, etc. Keyboard-driven operation remains central to tmux.

---

## 12. Plugins

Optional extensions, provided by external tools. They can add:

- session persistence
- clipboard integration
- themes
- status information
- additional commands

**Config vs plugins:** config changes how existing features behave (`~/.tmux.conf`); plugins add new functionality. tmux itself provides the core.

Recommended path:

```
understand core concepts → configure keybindings/appearance → add plugins only when needed
```

---

## 13. Environment and Working Directory

- tmux maintains environment info per session (`PATH`, `EDITOR`, `SHELL`, `PWD`).
- New panes/windows generally **inherit the environment** of their tmux context.
- New panes/windows can **inherit the current working directory**, useful for development:

```
~/projects/my-project → split pane → new pane starts in ~/projects/my-project
```

---

## 14. Terminal Capabilities

tmux sits between the terminal emulator and programs like Neovim:

```
Konsole → tmux → Neovim
```

It must correctly handle colors, cursor behavior, keyboard input, Unicode, and true color. Incorrect terminal settings can cause visual or keyboard problems.

---

## 15. Nested tmux

Running tmux inside tmux (`tmux → tmux → shell`) makes keybindings confusing because both instances use the same prefix. **Avoid unless there's a specific reason.**

---

## 16. Multiple Clients

Several terminals can attach to the same session, viewing or accessing it from different connections:

```
Konsole ──┐
          ├── tmux server ── Session
SSH ──────┘
```

---

## 17. Complete Mental Model

```
                         tmux server
                              │
                ┌─────────────┴─────────────┐
                │                           │
            Session A                   Session B
                │                           │
          ┌─────┴─────┐                 Window
          │           │                    │
       Window       Window                Pane
          │
      ┌───┴───┐
      │       │
    Pane     Pane
      │
    Bash
      │
   Neovim
```

```
Terminal emulator → tmux → Session → Window → Pane → Process
```

### Cheat summary

| Term | Meaning |
|---|---|
| tmux | terminal multiplexer |
| server | manages tmux state |
| client | terminal connected to server |
| session | persistent workspace |
| window | tab |
| pane | split |
| prefix | invokes tmux keybindings |
| attach | connect to session |
| detach | leave session without destroying it |
| kill | destroy session/window/pane |
| copy mode | navigate/copy terminal history |
| `.conf` | customize tmux |
