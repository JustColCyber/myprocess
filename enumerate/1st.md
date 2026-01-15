# 1st step in enumeration of a target

## Open Ports and Services

### NMAP for open TCP ports.

nmap -p- -sC -Pn 10.10.11.98 -open -v

### NMAP for open UDP ports.

nmap -Pn -sU -sV -T3 10.129.52.231

### NMAP for top 25 UDP ports

nmap -Pn -sU -sV -T3 --top-ports 25 10.129.95.215 -v

### NMAP for aggressive scan on a single port

nmap -A -p 80 -sV 10.10.11.98 -v

### NMAP Scripts

/usr/share/nmap/scripts

