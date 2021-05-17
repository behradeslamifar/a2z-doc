# tmux config
tmux manual and document expression
| keyword | Description |
| --- | --- |
| `C` | Ctrl  |
| `M` | Alt   |
| `prefix` | `ctrl`+`b` |


## my tmux.conf

```
# Grouping, synchronize all panes in a window
bind -n C-g setw synchronize-panes on
bind -n M-g setw synchronize-panes off

# unbind default prefix and set it to ctrl-a
unbind C-b
set -g prefix C-a
bind C-a send-prefix

# make delay shorter
set -sg escape-time 0

# pane movement shortcuts (same as vim)
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

#### key bindings ####
# reload config file
bind r source-file ~/.tmux.conf \; display ".tmux.conf reloaded!"

# quickly open a new window
bind N new-window

# enable mouse support for switching panes/windows
set -g mouse-utf8 on
set -g mouse on

#### copy mode : vim ####

# set vi mode for copy mode
setw -g mode-keys vi

# copy mode using 'Esc'
unbind [
bind Escape copy-mode

# start selection with 'space' and copy using 'y'
bind-key -T copy-mode-vi 'v' send-keys -X begin-selection
bind -T copy-mode-vi y send-keys -X copy-pipe-and-cancel 'xclip -in -selection clipboard'

# paste using 'p'
unbind p
bind p paste-buffer
```
