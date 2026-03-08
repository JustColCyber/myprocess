# Recon
Recon is required for some engagements, not all. If the scope is large and it is not clear what to enumerate then recon is required.

## Whois

```
sudo apt update
sudo apt install whois -y
whois example.com
```

## DNS

### dnsrecon

Comprehensive DNS enumeration, identifying subdomains, and gathering DNS records for further analysis.

### amass

Actively maintained tool focused on subdomain discovery, known for its integration with other tools and extensive data sources.

#### Passive

amass enum -passive -d owasp.org

#### Active

amass enum -active -d owasp.org -brute -w /root/dns_lists/deepmagic.com-top50kprefixes.txt -src -ip -dir amass4owasp -config /root/amass/config.ini -o amass_results_owasp.txt

### dnsdumpster.com

recon & research, find & lookup dns records

### dnsenum

Comprehensive DNS enumeration tool that supports dictionary and brute-force attacks for discovering subdomains.

#### censys.io for Cert analysis

censys.io

## Endpoints

Shodan.io

## Subdomain Passive

#### crt.sh for Cert Transperency Logs 

crt.sh to search for an organisation name and find endpoints and cert history.

```
curl -s "https://crt.sh/?q=example.com&output=json" | jq -r '.[] | select(.name_value | contains("dev")) | .name_value' | sort -u
```

## Google Dorking

Finding Login Pages:
    site:example.com inurl:login
    site:example.com (inurl:login OR inurl:admin)
    
Identifying Exposed Files:
    site:example.com filetype:pdf
    site:example.com (filetype:xls OR filetype:docx)
    
Uncovering Configuration Files:
    site:example.com inurl:config.php
    site:example.com (ext:conf OR ext:cnf) (searches for extensions commonly used for configuration files)
    
Locating Database Backups:
    site:example.com inurl:backup
    site:example.com filetype:sql****


## Web Archives

web.archive.org

## Recon Automated

### theharvester

Collecting email addresses, employee information, and other data associated with a domain from multiple sources.

You can include your Shodan.io API key.

### Final Recon

### Recon-ng

### SpiderFoot

### OSINT Framework

https://osintframework.com/
