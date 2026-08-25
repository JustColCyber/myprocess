Start a new session:

```
tmux
```

Start a new session and name it:

```
tmux new -s <session_name>
```

Detach from a session: 

Press Ctrl + b, release, and then press d to let your processes run in the background.

Reattach to a session to resume your work from any local or remote connection.
```
tmux attach
``` 
or 
```
tmux a
``` 

List existing sessions: Type 

```
tmux ls
```

Attach to a session in the list from outside TMUX.

```
tmux a -t <NAME of SESSION>
```

Attach to a session in the list from inside TMUX.

```
tmux a -t <NAME of SESSION>
```

Rename a session:

```
tmux rename-session -t old_name new_name
```

Stop a TMUX session from outside:
```
tmux kill-session -t session_name
```

Stop all TMUX sessions from outside:
```
tmux kill-server
```

Stop a TMUX session from inside:
1. Press Ctrl + b, then release.
2. Press : to open the tmux command prompt at the bottom of the screen.
3. Type kill-session and press Enter.