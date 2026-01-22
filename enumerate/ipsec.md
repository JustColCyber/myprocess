# IPSEC Enumeration

## NMAP Scanning IPSEC UDP Ports

map -A -p 500 -sU 10.1.1.1 -v

nmap -A -p 4500 -sU 10.1.1.1 -v

### IPSEC ports and networking
nmap -sU -Pn -T4 -p500 -A 10.1.1.1

## Ikescan

Show if IPSEC gateway, IPSEC vendor, and cipher suite negotiation info

ike-scan -M --showbackoff 10.1.1.1

Handshake scan to get the group ID and a hash

ike-scan -P -M -A -n fakeID 10.1.1.1
