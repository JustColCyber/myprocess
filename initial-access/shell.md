# Stablise Your Shell

Spawn a PTY (on Victim Shell):

python3 -c 'import pty; pty.spawn("/bin/bash")' 

You may have to type this blind after which The prompt will change.

Background and STTY (on Attacker Machine):
Press Ctrl+Z. This backgrounds netcat.
Type stty raw -echo; fg.
Press Enter.

Final Touches (on Victim Shell):
reset
export TERM=xterm
export SHELL=bash
