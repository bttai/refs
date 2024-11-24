
# Windows `netsh` Command Cheat Sheet

---

## Basic Concepts

- **netsh**: A command-line scripting utility to manage and monitor network configurations on Windows.
- Works with various contexts like `firewall`, `interface`, `routing`, and `wlan`.

---

## General Commands

### 1. Display Help
```cmd
netsh /?                     # Show general help
netsh <context> /?           # Show help for a specific context (e.g., `firewall`)
```

### 2. Switch Contexts
```cmd
netsh firewall               # Enter the firewall context
netsh interface ipv4         # Enter the IPv4 interface context
```

### 3. Exit the Utility
```cmd
exit                         # Exit the netsh utility
```

---

## Firewall Management

### 1. Enable/Disable Firewall
```cmd
netsh advfirewall set allprofiles state on      # Enable the firewall for all profiles
netsh advfirewall set allprofiles state off     # Disable the firewall for all profiles
```

### 2. Add a Rule
```cmd
netsh advfirewall firewall add rule name="Allow HTTP" protocol=TCP dir=in localport=80 action=allow
# Allow incoming HTTP traffic on port 80
```

### 3. Delete a Rule
```cmd
netsh advfirewall firewall delete rule name="Allow HTTP"
```

### 4. Show Rules
```cmd
netsh advfirewall firewall show rule name="Allow HTTP"
netsh advfirewall firewall show currentprofile
```

---

## Network Interface Configuration

### 1. Show Interfaces
```cmd
netsh interface show interface                 # List all interfaces
netsh interface ipv4 show addresses            # Display IPv4 address details
```

### 2. Set IP Address (Static)
```cmd
netsh interface ipv4 set address name="Ethernet" static 192.168.1.100 255.255.255.0 192.168.1.1
# Assign a static IP, subnet mask, and default gateway
```

### 3. Set IP Address (DHCP)
```cmd
netsh interface ipv4 set address name="Ethernet" source=dhcp
```

### 4. Set DNS Servers
```cmd
netsh interface ipv4 set dns name="Ethernet" static 8.8.8.8
netsh interface ipv4 add dns name="Ethernet" 8.8.4.4 index=2
```

---

## Wireless (Wi-Fi) Management

### 1. Show Wireless Profiles
```cmd
netsh wlan show profiles                       # List saved wireless profiles
```

### 2. View Profile Details
```cmd
netsh wlan show profile name="WiFiName" key=clear
# Display the details of a specific profile (with clear text key)
```

### 3. Export Profiles
```cmd
netsh wlan export profile key=clear folder="C:\Users\YourUser\Desktop"
# Export all profiles to a folder
```

### 4. Delete a Profile
```cmd
netsh wlan delete profile name="WiFiName"
```

---

## Routing and NAT

### 1. Show Routing Table
```cmd
netsh interface ipv4 show route
```

### 2. Add a Static Route
```cmd
netsh interface ipv4 add route 192.168.2.0/24 "Ethernet" 192.168.1.1
```

### 3. Delete a Static Route
```cmd
netsh interface ipv4 delete route 192.168.2.0/24 "Ethernet"
```

---

## Port Forwarding

### 1. Add a Port Forwarding Rule
```cmd
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=8080 connectaddress=192.168.1.100 connectport=80
# Forward traffic from local port 8080 to 192.168.1.100:80
```

### 2. Show Port Forwarding Rules
```cmd
netsh interface portproxy show v4tov4
```

### 3. Delete a Port Forwarding Rule
```cmd
netsh interface portproxy delete v4tov4 listenport=8080 listenaddress=0.0.0.0
```

---

## Useful Tips

- Run `netsh` as Administrator for full functionality.
- Use `netsh <context> show ...` commands to inspect configurations.
- Export firewall settings for backup or migration:
  ```cmd
  netsh advfirewall export "C:\Path\firewall-config.wfw"
  netsh advfirewall import "C:\Path\firewall-config.wfw"
  ```

---

## Helpful Resources

- `netsh` Help: Run `netsh /?` in a Command Prompt
- Microsoft Documentation: [Netsh Commands](https://learn.microsoft.com/en-us/windows-server/networking/technologies/netsh/netsh-contexts)

