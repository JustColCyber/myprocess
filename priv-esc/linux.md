Manual searching for Priv Esc vectors from the HTB Linux Priv Esc section: https://academy.hackthebox.com/app/module/51/section/1592

## OS Version

## Kernel Version

## Running Services

ps aux | grep root

## Open Ports


### Socket Statistics

ss -tulpn

## Installed Packages and Versions

## Logged in Users

## User Home Directories

## User's Home Directory Contents

## User's Home Directory Contents

## Bash History

## Sudo -l

List User privs.

## Configuration Files

## Readable Shadow File

## Password Hashes in /etc/passwd

cat /etc/passwd

## Cron Jobs

## File Systems & Additional Drives

lsblk

## Setuid and Setgid Perms

## Writable Directories

find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null

## Writable Files

find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null


