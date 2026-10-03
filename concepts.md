tmux Concepts
1. What is tmux?

tmux = terminal multiplexer.

It manages multiple terminal environments inside one terminal and keeps them organized and persistent.

Konsole
└── tmux
    └── Session
        ├── Window
        │   ├── Pane
        │   └── Pane
        └── Window
2. Terminal Emulator vs tmux
Konsole → terminal emulator
tmux    → terminal multiplexer

Konsole provides the terminal window.

tmux manages terminal sessions, windows, and panes inside it.

3. tmux Server

When tmux starts, a tmux server manages the tmux state.

tmux server
├── Session
├── Session
└── Session

The server owns sessions, windows, panes, and their running processes.

This is what allows sessions to survive after detaching.

4. tmux Client

A client is the terminal interface connected to the tmux server.

Konsole
   │
tmux client
   │
tmux server
   │
sessions

Multiple clients can connect to the same server/session.

5. Session

A session is a persistent workspace.

Session: c-project
├── Window: editor
├── Window: terminal
└── Window: git

Use sessions to separate projects or tasks.

lua
linux-lab
c-project
neovim
Important

Detaching from a session does not destroy it.

Ctrl+b d
6. Window

A window is similar to a terminal tab.

Session
├── Window 1
├── Window 2
└── Window 3

Each window has its own layout and panes.

Use windows for different working contexts.

Example:

1:editor
2:terminal
3:git
7. Pane

A pane is a split terminal area inside a window.

┌──────────────────┬──────────────────┐
│                  │                  │
│     Neovim       │      Bash        │
│                  │                  │
└──────────────────┴──────────────────┘

A window can contain many panes.

8. The Hierarchy

The fundamental tmux model:

tmux server
    ↓
Session
    ↓
Window
    ↓
Pane

Remember:

Session = workspace
Window  = tab
Pane    = split
9. Prefix Key

tmux uses a prefix key to distinguish tmux commands from normal terminal input.

Default:

Ctrl+b

Example:

Ctrl+b c

means:

Press Ctrl+b
Release
Press c
10. Attach and Detach
Attach

Connect to an existing session.

tmux attach
Detach

Leave the session while keeping it alive.

Ctrl+b d
Attached
   ↓
Detach
   ↓
Session continues
   ↓
Attach again
11. Kill

Killing is different from detaching.

Detach
Session remains alive
Kill
Session and its contents are destroyed

Example:

tmux kill-session -t name
12. Persistence

tmux keeps the terminal environment managed by its server alive after the client disconnects.

Example:

SSH
 ↓
tmux
 ↓
long-running process

SSH disconnects:

SSH ✕
tmux ✓
process ✓

Reconnect and attach to the session.

13. SSH Use Case

tmux is especially useful on remote machines.

Laptop
  │
  │ SSH
  ▼
Remote server
  │
  └── tmux
       ├── Neovim
       ├── shell
       └── process

The tmux session can continue while the SSH connection is temporarily gone.

14. Processes and Panes

A pane normally contains a shell or another terminal program.

Example:

Pane
└── Bash
    └── Neovim

If the shell/process in a pane exits, that pane can close.

tmux does not make every program immortal; it preserves the tmux workspace and processes managed within it while the server remains alive.

15. Windows and Panes Are Independent

Each window has its own pane layout.

Window 1
├── Pane
└── Pane

Window 2
└── Pane

Window 3
├── Pane
├── Pane
└── Pane

Switching windows changes the entire workspace being displayed.

Switching panes changes the active area within the current window.

16. Layouts

tmux automatically arranges panes.

Common layouts include:

even-horizontal
even-vertical
main-horizontal
main-vertical
tiled

Layouts control how panes occupy the window.

17. Pane Zoom

A pane can temporarily occupy the entire window.

Normal:
┌──────────┬──────────┐
│          │          │
│  Pane    │  Pane    │
└──────────┴──────────┘

Zoom:
┌─────────────────────┐
│                     │
│      Pane           │
│                     │
└─────────────────────┘

Zooming changes the display, not the underlying layout.

18. Copy Mode

tmux has a copy mode for navigating and copying terminal history.

It allows you to:

scroll through output
search terminal history
select text
copy text

It is useful when normal terminal scrolling isn't enough.

19. Status Bar

The status bar normally appears at the bottom.

It can display:

Session | Windows                         Time

Example:

c-project   1:editor  2:terminal  3:git       12:30

It can be customized with colors, information, layouts, and keybindings.

20. Configuration

tmux reads configuration from:

~/.tmux.conf

Configuration can change:

keybindings
colors
status bar
pane borders
mouse support
window numbering
terminal behavior
default commands

Configuration can be reloaded without restarting the server.

21. Keybindings

tmux commands can be:

Prefix bindings
Ctrl+b + key
Custom bindings

Defined in:

~/.tmux.conf

Example:

bind h select-pane -L

This makes:

Ctrl+b h

move left.

22. Commands vs Keybindings

tmux has two main ways of interacting with it.

Shell commands
tmux ls
tmux new -s project
tmux attach -t project
Inside-tmux keybindings
Ctrl+b c
Ctrl+b d
Ctrl+b %

Shell commands are useful for managing tmux from outside.

Keybindings are useful while working inside tmux.

23. Targets

Many tmux commands can specify a target session, window, or pane.

Example:

tmux kill-session -t project

Here:

-t project

identifies the target.

Targets become useful when managing many sessions/windows/panes.

24. Session Naming

Sessions can be named:

tmux new -s c-project

Names make multiple sessions easier to identify.

c-project
lua-learning
linux-lab
25. Window Naming

Windows can also be named:

1:editor
2:terminal
3:git

Names make the status bar easier to understand.

26. Pane Identification

tmux internally identifies panes with numbers.

Example:

0:0.0

Conceptually:

session : window . pane

This allows commands to target specific panes.

27. Environment Variables

tmux maintains environment information for sessions.

This can affect variables such as:

PATH
EDITOR
SHELL
PWD

A new pane/window generally inherits the environment of its tmux context.

28. Current Working Directory

New windows and panes can inherit the current working directory.

This is particularly useful for development:

~/projects/my-project

Split the pane:

new pane → ~/projects/my-project

instead of starting somewhere unrelated.

29. Mouse Support

tmux can optionally support the mouse.

It can provide:

pane selection
pane resizing
window selection
scrolling
other mouse interactions

Keyboard-driven operation remains central to tmux.

30. Terminal Capabilities

tmux sits between the terminal emulator and programs such as Neovim.

Konsole
   ↓
tmux
   ↓
Neovim

tmux must correctly handle terminal capabilities such as:

colors
cursor behavior
keyboard input
Unicode
true color

Incorrect terminal settings can cause visual or keyboard problems.

31. Plugins

tmux supports extensions through external tools/plugins.

Plugins can provide things such as:

session persistence
clipboard integration
themes
status information
additional commands

Plugins are optional.

tmux itself provides the core functionality.

32. Configuration vs Plugins

A configuration changes how tmux's existing features behave.

~/.tmux.conf

A plugin adds additional functionality.

A good starting point is:

tmux
   ↓
understand core concepts
   ↓
configure keybindings/appearance
   ↓
add plugins only when needed
33. Nested tmux

It is possible to run tmux inside another tmux session:

tmux
└── tmux
    └── shell

This can make keybindings confusing because both tmux instances use the prefix.

Nested tmux should generally be avoided unless there is a specific reason to use it.

34. Multiple Clients

Multiple terminal clients can attach to the same tmux session.

Conceptually:

Konsole ──┐
          ├── tmux server ── Session
SSH ──────┘

This allows the same session to be viewed or accessed from different connections.

35. tmux's Core Purpose

tmux primarily solves three problems:

1. Organization
Sessions
 └── Windows
      └── Panes
2. Persistence
Detach
  ↓
Session continues
3. Remote workflows
SSH disconnects
      ↓
tmux session remains
      ↓
Reconnect
      ↓
Attach
36. The Complete Mental Model
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

The core ideas to remember:

tmux        = terminal multiplexer
server      = manages tmux state
client      = terminal connected to server
session     = persistent workspace
window      = tab
pane        = split
prefix      = invokes tmux keybindings
attach      = connect to session
detach      = leave session without destroying it
kill        = destroy session/window/pane
copy mode   = navigate/copy terminal history
.conf       = customize tmux

Core mental model:

Terminal emulator
       ↓
      tmux
       ↓
    Session
       ↓
    Window
       ↓
     Pane
       ↓
    Process
