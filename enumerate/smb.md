REFS:

https://academy.hackthebox.com/app/module/116/section/1167

Initially, it was designed to run on top of NetBIOS over TCP/IP (NBT) using TCP port 139 and UDP ports 137 and 138. However, with Windows 2000, Microsoft added the option to run SMB directly over TCP/IP on port 445 without the extra NetBIOS layer. 

## Enumerate

```
nmap 10.129.14.128 -sV -sC -p139,445
```

## SMB Client

The option -N uses the null session.
```
smbclient -N -L //10.129.14.128
```
List the files in the directory using the Null session:

```
smbclient -N //10.129.230.181/support-tools -c 'dir'
```
Download files using the Null session:

smbclient -N //10.129.230.181/support-tools -c 'get filename'

## SMB MAP

```
smbmap -H 10.129.14.128
```

```
smbmap -H 10.129.14.128 -r sharename
```

Download a file.

```
smbmap -H 10.129.14.128 --download "sharenam\file.txt"
```

Upload a file.
```
smbmap -H 10.129.14.128 --upload test.txt "notes\test.txt"
```