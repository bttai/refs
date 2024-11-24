
# SMB Enumeration Guide

## 1. Discovery

### Find SMB Hosts
- **Using Nmap**:
  ```bash
  nmap -v -p 139,445 -oG smb.txt 192.168.50.1-254
  ```

- **Using Nbtscan**:
  ```bash
  sudo nbtscan -r 192.168.50.0/24
  ```

- **Using Crackmapexec**:
  ```bash
  sudo crackmapexec smb 192.168.50.0/24
  ```

## 2. Basic Enumeration

### Identify SMB Version and OS
- **Using Nmap Scripts**:
  ```bash
  nmap -v -p 139,445 --script smb-os-discovery <IP>
  ```

- **Using Enum4Linux**:
  ```bash
  enum4linux <IP>
  ```

- **Using Net View on Windows**:
  ```cmd
  net view \\<ComputerName> /all
  ```

## 3. List Shares

### Enumerate Shared Folders
- **Using smbclient (Null Session)**:
  ```bash
  smbclient --no-pass -L //<IP>
  ```

- **Using smbmap**:
  ```bash
  smbmap -H <IP>
  smbmap -u <USER> -p <PASS> -H <IP>
  ```

- **Using Crackmapexec**:
  ```bash
  crackmapexec smb <IP> -u '' -p '' --shares
  ```

## 4. Access Shares

### Connect to Shared Folders
- **Using smbclient**:
  ```bash
  smbclient --no-pass //<IP>/<Share>
  smbclient -U '<USER>[%<PASS>]' //<IP>/<Share>
  ```

- **Recursive Listing**:
  ```bash
  smbclient --no-pass //<IP>/<Share> -c 'recurse;ls'
  ```

- **Using Impacket's smbclient**:
  ```bash
  impacket-smbclient anonymous:anonymous@<IP>
  impacket-smbclient -hashes <NTLM_HASH> <DOMAIN>/<USER>@<IP>
  ```

## 5. Recursive Share Enumeration

### Explore Share Contents
- **Using smbmap**:
  ```bash
  smbmap -u <USER> -p <PASS> -R <Share> -H <IP>
  ```

## 6. Advanced Techniques

### Kerberos Authentication
- **smbclient with Kerberos**:
  ```bash
  smbclient -k //<IP>/<Share> -U <DOMAIN>/<USER>
  ```

### Set Up Your SMB Server
- **Using Impacket**:
  ```bash
  impacket-smbserver <Share_name> /path/to/<Share> -username <USER> -password <PASS>
  ```

## 7. Detection and Hardening

### Detection
- Monitor logs for SMB enumeration attempts (e.g., Windows Event Viewer, Sysmon).

### Hardening
- Disable null sessions.
- Audit share permissions.
- Enforce strong passwords.
- Use firewalls to restrict SMB traffic.

---

## Checklist for Enumeration
1. Scan for SMB hosts.
2. Enumerate shares (null/guest/credentials).
3. List shares contents recursively.
4. Try credentials, hashes, or Kerberos.
5. Explore accessible files for sensitive data.

---
