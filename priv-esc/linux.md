Manual searching for Priv Esc vectors from the HTB Linux Priv Esc section: https://academy.hackthebox.com/app/module/51/section/1592

## OS Version

cat /etc/os-release

## Kernel Version

cat /proc/version

uname -a

lscpu

## Running Services

ps aux | grep root

## Open Ports


### Socket Statistics

ss -tulpn

## Installed Packages and Versions

## Users

Shared resources:
```
id user1
id user2
groups user1
groups user2
```
```
find / -user user2 -type f 2>/dev/null
find / -user user2 -type d 2>/dev/null
find / -group user2 -type f 2>/dev/null 2>&1

```
Files both users can reach:
```
# files owned by user2 that user1's group can read/write
find / -user user2 \( -perm -g+r -o -perm -g+w \) 2>/dev/null

# anything in user2's home readable by group user1 or world
ls -la /home/user2 2>/dev/null
find /home/user2 -perm -o+r 2>/dev/null
find /home/user2 -perm -g+r 2>/dev/null

```
or/and
```
# any file/dir on disk owned by a group that both user1 and user2 are in
for g in $(id -Gn user1) $(id -Gn user2); do
  echo "== group: $g =="
  find / -group "$g" 2>/dev/null
done | sort -u

```

## User Home Directories

## User's Home Directory Contents

## User's Home Directory Contents

## Bash History

history

## Users Paths

echo $PATH

## Root testing

Sudo -l

List User privs.

### SUID Binaries, check for scripts invoking them

[asterisk@connected asterisk]$ find / -perm -4000 -type f 2>/dev/

### CRON jobs running as root?

cat /etc/crontab /etc/cron.d/* 2>/dev/null
ls -la /var/spool/cron/crontabs/ 2>/dev/null

## Groups

groups 
```
cat /etc/group
```
```
id
cat /etc/group | grep user1
getent group user1
```

```
find / -group <GROUP NAME> -type f 2>/dev/null
find / -group <GROUP NAME> -type d 2>/dev/null

```
## Binaries

```
find / -perm -4000 -type f 2>/dev/null
find / -perm -2000 -type f 2>/dev/null

```
World writable binaries

```
ls -la /etc/cron* 2>/dev/null
cat /etc/crontab 2>/dev/null
find / -writable -type f 2>/dev/null | grep -v /proc
cat /etc/passwd

```

## Shells available

cat /etc/shells

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


