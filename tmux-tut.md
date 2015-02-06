---
layout: page
title:  "Tmux Tutorial"
categories: how-to
---

# tmux configuration

```
set -g prefix C-j
set -g status-left-length 70
set -g status-left "#[fg=green]| #h |"

# show session name, window & pane number, date and time on right side of
# status bar
set -g status-right-length 60
set -g status-right "#[fg=blue]#S #I:#P #[fg=yellow]| %d %b %Y #[fg=green]| %l:%M %p |"
```

# Commands

| Session Commands | Definition |
|------------------|------------|
| `tmux ls` | show current sessions |
| `tmux attach -t billy`    | attach to a session named 'billy' |
| `tmux new -s billy`   | create a new session named 'billy' |



| In Tmux Commands | Definition |
|------------------|------------|
| `Cap-j d` | detach from session |


| Window Commands  | Definition |
|------------------|------------|
| `Cap-j c`     | create a new window |
| `Cap-j ,`     | rename the current window |
| `Cap-j n`     | move to next window |
| `Cap-j p`     | move to previous window |
| `Cap-j w`     | list the windows |


|  Pane Commands   | Definition |
|------------------|------------|
| `Cap-j %`     | split vertically |


|  Command Commands   | Definition |
|------------------|------------|
| `Cap-j :`     | prompt to enter commands |
