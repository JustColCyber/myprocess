# 1st step in enumeration of a target

## Open Ports and Services

### NMAP for open ports.

nmap -p- -sC -Pn 10.10.11.98 -open -v

### NMAP for aggressive scan on a single port

nmap -A -p 80 -sV 10.10.11.98 -v

### NMAP Scripts

/usr/share/nmap/scripts

