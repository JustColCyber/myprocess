# 1st step in enumeration of a target

This is to see what ports are open.

## Open Ports and Services

### NMAP for open TCP ports.

nmap -p- -sC -Pn 10.10.11.98 -open -v

### NMAP for open UDP ports.

nmap -Pn -sU -sV -T3 10.10.11.98

#### NMAP for specific UDP ports.

nmap -sU -p 53,161,500 10.10.11.98

nmap -sU -Pn -T4 -p500 -A 110.10.11.98

#### NMAP for top 25 UDP ports

nmap -Pn -sU -sV -T3 --top-ports 25 10.10.11.98 -v

### NAMP the open ports for more info.

sudo nmap -p 22,80,110,111,143,993,995,2049,42423,53415,57621,57855 -sC -sV 10.10.11.98 -v

### NMAP for aggressive scan on a single UDP port

nmap -A -p 80 -sU 10.10.11.98 -v

## Version Detection

### NMAP the versions for the discovered services

Intensitity values are 0 - 9

sudo nmap -sV --version-intensity 5 10.10.11.98


### NMAP Aggressive on particular ports

nmap -A -p 143,993,110,995 -sV 10.10.11.98 -v 



## NMAP Scripts

/usr/share/nmap/scripts

