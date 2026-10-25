REFS:

https://academy.hackthebox.com/app/module/67

## Privs

whoami /priv

## Groups

whoami /groups

## winpeas

REF: https://github.com/peass-ng/PEASS-ng/tree/master/winPEAS

##cmd

whoami /all

reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" 2>nul | findstr /i "DefaultUserName DefaultPassword AutoAdminLogon"

reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

Get-CimInstance Win32_Service | ? {$_.PathName -notmatch '"' -and $_.PathName -match ' ' -and $_.PathName -notmatch 'C:\\Windows'} | select Name,PathName,StartName