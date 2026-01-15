# Common Services

## SSH

nmap -A -p 22 -sV 10.10.11.87 -v

nmap --script banner.nse 10.10.11.87   

map -v -sT --script=ssh-auth-methods.nse -p 22 192.168.29.143 -o nmap-output-auth-methods.txt

telnet 10.10.11.87 22

nc 10.10.11.87 22

See the auth methods that are supported.

nmap -v -sT --script=ssh-auth-methods.nse -p 22 10.10.11.87

```
PORT   STATE SERVICE
22/tcp open  ssh
| ssh-auth-methods: 
|   Supported authentication methods: 
|     publickey
|_    password
```
