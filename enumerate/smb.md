REFS:

https://academy.hackthebox.com/app/module/116/section/1167

Initially, it was designed to run on top of NetBIOS over TCP/IP (NBT) using TCP port 139 and UDP ports 137 and 138. However, with Windows 2000, Microsoft added the option to run SMB directly over TCP/IP on port 445 without the extra NetBIOS layer. 

```
nmap 10.129.14.128 -sV -sC -p139,445
```

```
smbmap -H 10.129.14.128
```

```
smbmap -H 10.129.14.128 -r sharename
```

```
smbmap -H 10.129.14.128 --download "sharenam\file.txt"
```