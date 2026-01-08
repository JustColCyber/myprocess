# Process to enumerate a web service

## Service Identificaiton

### NMAP for service identification.

nmap -p 80 -sV 10.10.11.98 -v

### NMAP Banner Grabbing

nmap --script banner.nse 10.10.11.98

### NMAP Aggressive Scannign

nmap -A -p 80 -sV 10.10.11.98 -v 

## Directory Enumeration

### dirsearch

dirsearch -u http://monitorsfour.htb -x 404

### FFUF

ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-lowercase-2.3-big.txt -u http://monitorsfour.htb/FUZZ

## Sub-Domain Enumeration

ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://10.10.11.98 -H "Host: FUZZ.monitorsfour.htb" -c -fs 138

## Stack Enumeration

Wapalyzer
