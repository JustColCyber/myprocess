REFS:

https://academy.hackthebox.com/app/module/67

## Privs

whoami /priv

## Groups

whoami /groups

## findstr for "password"

findstr /S /I /C:"password" "C:\Program Files\*"

## findstr for "password" in specific files

findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml

## winpeas

REF: https://github.com/peass-ng/PEASS-ng/tree/master/winPEAS

## cmd

whoami /all

reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" 2>nul | findstr /i "DefaultUserName DefaultPassword AutoAdminLogon"

reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

Get-CimInstance Win32_Service | ? {$_.PathName -notmatch '"' -and $_.PathName -match ' ' -and $_.PathName -notmatch 'C:\\Windows'} | select Name,PathName,StartName

## Powershell

### History File
Starting with Powershell 5.0 in Windows 10, PowerShell stores command history to the file:

C:\Users\<username>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt.

Confirm the location of the history file:

```
(Get-PSReadLineOption).HistorySavePath
```


