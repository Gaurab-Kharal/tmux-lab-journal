# tmux Personal Cheatsheet

Custom settings and bindings from my `~/.tmux.conf`.
Default tmux keys are in `cheatsheet.md`.

Prefix is still the default: `Ctrl+b`.

---

## Custom Keybindings

| Action                               | Key                    | Notes                        |
| ------------------------------------ | ---------------------- | ---------------------------- |
| Pane left / down / up / right        | `Ctrl+b h` `j` `k` `l` | Vim-style                    |
| Resize pane left / down / up / right | `Ctrl+b H` `J` `K` `L` | 5 cells; **repeatable**      |
| Split side by side                   | `Ctrl+b \|`            | Keeps current directory      |
| Split top / bottom                   | `Ctrl+b -`             | Keeps current directory      |
| New window                           | `Ctrl+b c`             | Now keeps current directory  |
| Last window                          | `Ctrl+b Tab`           | Replaces default `Ctrl+b l`  |
| Reload config                        | `Ctrl+b r`             | Shows "tmux config reloaded" |

**Repeatable resize:** press `Ctrl+b H` once, then tap `H` again without the prefix. The window is about 500 ms by default; stop pressing and it ends.

---

## Defaults I Changed or Overrode

| Default key | Original action       | Now                                      |
| ----------- | --------------------- | ---------------------------------------- |
| `Ctrl+b l`  | Last window           | Pane right (last window is `Ctrl+b Tab`) |
| `Ctrl+b L`  | Switch to last client | Resize pane right                        |
| `Ctrl+b -`  | Delete buffer         | Split top / bottom                       |
| `Ctrl+b r`  | Refresh client        | Reload config                            |
| `Ctrl+b c`  | New window            | New window in current directory          |

Default splits still work: `Ctrl+b %` and `Ctrl+b "` (but they don't keep the current directory).
Arrow-key pane movement (`Ctrl+b Arrow`) still works too.

---

## Behavior Changes

| Setting                          | Effect                                                         |
| -------------------------------- | -------------------------------------------------------------- |
| `base-index 1`                   | Windows start at **1**, not 0. Select with `Ctrl+b 1`–`9`      |
| `pane-base-index 1`              | Panes start at 1 (`Ctrl+b q` shows them)                       |
| `renumber-windows on`            | Closing a window renumbers the rest, no gaps                   |
| `mouse on`                       | Click panes/windows, drag borders to resize, scroll with wheel |
| `escape-time 10`                 | Little delay after `Esc` (good for Neovim)                     |
| `history-limit 50000`            | 50,000 lines of scrollback (default 2000)                      |
| `focus-events on`                | Neovim gets focus in/out events                                |
| `mode-keys vi`                   | Vim keys in copy mode                                          |
| `default-terminal tmux-256color` | Correct `TERM` inside tmux                                     |
| `terminal-features ...:RGB`      | True color enabled                                             |

---

## Copy Mode (vi keys)

Enter with `Ctrl+b [`

| Action                     | Key                 |
| -------------------------- | ------------------- |
| Move                       | `h` `j` `k` `l`     |
| Half page up / down        | `Ctrl+u` / `Ctrl+d` |
| Top / bottom of history    | `g` / `G`           |
| Search forward / backward  | `/` / `?`           |
| Next / previous match      | `n` / `N`           |
| Start selection            | `Space`             |
| Copy selection and exit    | `Enter`             |
| Toggle rectangle selection | `v`                 |
| Quit                       | `q`                 |

Paste the tmux buffer: `Ctrl+b ]`

### Mouse and clipboard

- Plain drag → selects inside tmux (goes to the tmux buffer).
- **Hold `Shift` while dragging** → Konsole's own selection (system clipboard).

---

## Theme

- Dark purple, Neovim-style; the bar background is transparent through Konsole.
- Left: session name. Middle: windows (active one highlighted). Right: time and date.
- Icons need a **Nerd Font** in Konsole. They are written as `\uf120` and `\uf111` escapes in the config, so they survive copy-paste.
- Long session names are fine: `status-left-length` is 40.

---

## Config Management

```bash
# Reload inside tmux
Ctrl+b r

# Reload from shell
tmux source-file ~/.tmux.conf

# Check a setting
tmux show -g default-terminal

# Full restart (needed for some terminal settings)
tmux kill-server
```

`~/.tmux.conf` should be a symlink to the repo copy:

```bash
ln -sf ~/projects/learning-projects/tmux-lab-journal/tmux.conf ~/.tmux.conf
```

---

## Reminder: Remote Machines

On SSH servers none of this exists. `Ctrl+b h/j/k/l`, `|`, `-`, `Ctrl+b r` and the vi copy mode won't work there.
Keep practicing the defaults: `%`, `"`, `Arrow`, `c`, `n`, `p`, `d`.
