## LDAP Utils

### Install

apt install ldap-utils

### Ldapsearch
```
ldapsearch -H ldap://example.com-D ldap@sexample.com -w 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -b "dc=name1,dc=name" "*"
```

## Domain Users add Computers to the Domain

```
Get-ADObject -Identity ((Get-ADDomain).distinguishedname) -Properties ms-DS-
MachineAccountQuot
```

## Resource-Based Constrained Delegation

## Powerview

https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1

Import the Powershell module on the victim:
```
. ./PowerView.ps1
```

## Powermad

https://github.com/Kevin-Robertson/Powermad

## Rubeus

https://github.com/GhostPack/Rubeus
