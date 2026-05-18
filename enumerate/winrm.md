## Evil-winrm

Connect.

```
evil-winrm -u <username> -p '<PASSWORD>>' -i example.com
```
### File transfers

Upload a file after you are connected. On your host save the files you want to transfer to the diretory from which you started Evil-winrm.

```
upload file.txt
```

```
download file.txt 
```

### check the value of the ms-ds-machineaccountquota attribute

```
Get-ADObject -Identity ((Get-ADDomain).distinguishedname) -Properties ms-DS-MachineAccountQuota
```