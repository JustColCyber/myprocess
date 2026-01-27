# Process to enumerate a web service

## Service Identificaiton

### NMAP for service identification.

nmap -p 80 -sV 10.10.11.98 -v

### NMAP Banner Grabbing

nmap --script banner.nse 10.10.11.98

### NMAP Aggressive Scannign

nmap -A -p 80 -sV 10.10.11.98 -v 

## Stack Enumeration

### Whatweb

whatweb monitorsfour.htb

### EyeWitness

cd /usr/share/eyewitness
./EyeWitness.py --single http://soulmate.htb

Specify the output file:

./EyeWitness.py --single http://soulmate.htb -d /home/user1/soulmate

Use a NMAP xml to import the targets.
./EyeWitness.py -x nmap_scan.xml


### Wapalyzer

Browser Extension, click, look, and download output.

### Manual

curl and grep.

## Directory Enumeration

### dirsearch

dirsearch -u http://monitorsfour.htb -x 404

Specify the output path:

sudo dirsearch -u http://soulmate.htb -x 404 -o /home/user1/dirsearch/soulmate

### FFUF

ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-lowercase-2.3-big.txt -u http://monitorsfour.htb/FUZZ

## Sub-Domain Enumeration

Filter out a specific response size with -fs

ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://10.10.11.98 -H "Host: FUZZ.monitorsfour.htb" -c -fs 138

Filter out a specific response code with -fc 

ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://10.10.11.98 -H "Host: FUZZ.monitorsfour.htb" -c -fc 301

## XXE Enumeration

See the backend web tech.

```
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:output method="html" indent="yes" />

  <xsl:template match="/">
    <html>
        <body>
          <h1>System Fingerprint</h1>
          <ul>
            <li><b>Vendor:</b> <xsl:value-of select="system-property('xsl:vendor')"/></li>
            <li><b>Vendor URL:</b> <xsl:value-of select="system-property('xsl:vendor-url')"/></li>
            <li><b>XSLT Version:</b> <xsl:value-of select="system-property('xsl:version')"/></li>
          </ul>
        </body>
      </html>
  </xsl:template>
</xsl:stylesheet>
```
