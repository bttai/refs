xfreerdp /u:$USER /p:$PASS /v:$IP /d:$DOMAIN /cert-ignore
rdesktop -i $IP -u $USER -p $PASS


# Activate
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f

netsh advfirewall firewall set rule group=" remote desktop" new enable=yes


Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server'-name "fDenyTSConnections" -Value 0
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"


HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server



reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 1 /f

