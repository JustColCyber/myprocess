# IPSEC Enumeration

## NMAP Scanning IPSEC UDP Ports

map -A -p 500 -sU 10.129.95.215 -v

nmap -A -p 4500 -sU 10.129.95.215 -v

### IPSEC ports and networking
nmap -sU -Pn -T4 -p500 -A 110.10.11.98

## Ikescan

Show if IPSEC gateway, IPSEC vendor, and negotiation info

ike-scan -M --showbackoff 10.129.95.215
