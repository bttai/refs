
# SMB Enumeration Guide

## 1. Discovery

### Find SMB Hosts
- **Using Nmap**:
  ```bash
  nmap -v -p 139,445 -oG smb.txt 192.168.50.1-254
  nmap -v -p 139,445 --script smb-os-discovery <IP>
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
  ```powershell
  net view \<ComputerName> /all
  ```
## 3. Username Enumeration

  ```bash
    # CrackMapExec
    crackmapexec smb <IP> --users
    # Impacket's rpcclient
    rpcclient -U "" <IP>
    enumdomusers
    # Impacket's lookupsid
    lookupsid.py ''@<IP>
    # Metasploit has auxiliary modules for SMB user enumeration
    use auxiliary/scanner/smb/smb_enumusers
    set RHOSTS <IP>
    run
  ```

## 3. List Shares

### Enumerate Shared Folders
- **Using smbclient (Null Session)**:
  ```bash
  smbclient --no-pass -L //<IP>
  smbclient -U '<USER>[%<PASS>]' -L //<IP>
  ```

- **Using smbmap**:
  ```bash
  smbmap -H <IP>
  smbmap -u <USER> -p <PASS> -H <IP>
  ```

- **Using Crackmapexec**:
  ```bash
  crackmapexec smb <IP> -u '' -p '' --shares
  crackmapexec smb <IP> -u '<USER>' -p '<PASS>' --shares
  crackmapexec smb <IP> -u '<USER>' -H '<HASH>' --shares
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
  smbclient //<IP>/<Share> -U '<USER>[%<PASS>]' -c 'recurse;ls'
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

- **Recursively Download Files with smbclient**:
  ```bash
  smbclient //<IP>/<Share> -U '<USER>[%<PASS>]' -c 'mask ""; recurse on; prompt off; mget *'
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

### Mounting Shares on Linux
- **Mounting a Share**:
  ```bash
  mount -t cifs //<IP>/<Share> /mnt/<MountPoint>
  mount -t cifs -o "username=<USER>,password=<PASS>" //<IP>/<Share> /mnt/<MountPoint>
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

## 8. Additional Tips

- **Enumerating SMB Shares with Metasploit**:

  ```bash
    use auxiliary/scanner/smb/smb_enumshares
    set RHOSTS <IP>
    run
  ```
- **Using smbmap to find writable shares:**:

  ```bash
  smbmap -H <IP> -u <USER> -p <PASS> --check-write
  ```
- **Common SMB Misconfigurations to Look For**:

- Writable shares open to "Everyone."
- Misplaced sensitive files (passwords.txt, .git folders, etc.).
- Null or guest sessions allowing access.

- **Logging SMB Activity on Linux**:
  ```bash
    impacket-smbserver <Share_name> /path/to/<Share> -username <USER> -password <PASS> -debug
  ```
- **Reference for File Transfer with smbclient**:

  ```bash
    smbclient //<IP>/<Share> -U '<USER>'
    smb: \> get sensitive_file.txt
    smb: \> put exploit.exe
  ```
---

## Checklist for Enumeration
1. Scan for SMB hosts.
2. Enumerate shares (null/guest/credentials).
3. List shares contents recursively.
4. Try credentials, hashes, or Kerberos.
5. Explore accessible files for sensitive data.
