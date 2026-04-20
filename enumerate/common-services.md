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
## Docker

### Inside a Container

Look for entries with types like ext4, xfs, or overlay that point to directories you recognize. Bind mounts from the host often appear as regular device mounts but are mapped to your specific container paths.
```
mount
```
or grep
```
mount | grep -v ' /etc/' | grep -v ' /proc' | grep -v ' /sys'
```
or
```
findmnt
```

