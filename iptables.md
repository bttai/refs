
# iptables and nftables Cheat Sheet

---

## Basic Concepts

- **iptables**: A user-space utility to configure Linux kernel firewall (netfilter). Works with tables like `filter`, `nat`, `mangle`, and chains like `INPUT`, `OUTPUT`, `FORWARD`.
- **nftables**: The successor to iptables, providing a simpler, unified, and more efficient framework for packet filtering and NAT.

---

## iptables Commands

### 1. Listing Rules
```bash
iptables -L                  # List rules in the filter table
iptables -L -v -n            # List with packet counters, bytes, and IPs
iptables -t nat -L           # List rules in the NAT table
```

### 2. Flushing Rules
```bash
iptables -F                  # Flush all rules in the default table
iptables -t nat -F           # Flush NAT rules
iptables -X                  # Delete all custom chains
```

### 3. Adding Rules
```bash
iptables -A INPUT -p tcp --dport 80 -j ACCEPT   # Allow HTTP traffic
iptables -A INPUT -s 192.168.1.100 -j DROP      # Block traffic from an IP
iptables -A OUTPUT -p icmp -j ACCEPT            # Allow outgoing ping
```

### 4. Deleting Rules
```bash
iptables -D INPUT 1                             # Delete the first rule in the INPUT chain
iptables -D INPUT -p tcp --dport 80 -j ACCEPT   # Delete a specific rule
```

### 5. Default Policies
```bash
iptables -P INPUT DROP        # Set default INPUT policy to DROP
iptables -P OUTPUT ACCEPT     # Set default OUTPUT policy to ACCEPT
iptables -P FORWARD DROP      # Set default FORWARD policy to DROP
```

### 6. Saving and Restoring
```bash
iptables-save > /etc/iptables/rules.v4     # Save current rules
iptables-restore < /etc/iptables/rules.v4 # Restore saved rules
```

---

## nftables Commands

### 1. Listing Rules
```bash
nft list ruleset                      # Show all rules in the ruleset
nft list table ip filter              # Show rules in a specific table
```

### 2. Creating a Basic Ruleset
```bash
nft add table ip filter               # Create a new table
nft add chain ip filter input { type filter hook input priority 0; }  # Add an input chain
nft add rule ip filter input iifname "eth0" tcp dport 80 accept       # Allow HTTP
```

### 3. Adding Rules
```bash
nft add rule ip filter input ip saddr 192.168.1.100 drop  # Block traffic from an IP
nft add rule ip filter output icmp type echo-request accept # Allow outgoing ping
```

### 4. Deleting Rules
```bash
nft delete rule ip filter input handle 10  # Delete a rule by its handle
```

### 5. Flushing Rules
```bash
nft flush ruleset                         # Flush all rules
nft flush chain ip filter input           # Flush rules in a specific chain
```

### 6. Default Policies
```bash
nft add chain ip filter input { type filter hook input priority 0; policy drop; } # Set default policy to DROP
```

### 7. Saving and Restoring
```bash
nft list ruleset > /etc/nftables.conf     # Save rules to a file
nft -f /etc/nftables.conf                 # Restore rules from a file
```

---

## iptables vs nftables

| Feature                 | iptables                      | nftables                      |
|-------------------------|-------------------------------|-------------------------------|
| Syntax                  | Complex                      | Simpler                      |
| Performance             | Less efficient               | More efficient               |
| IPv4/IPv6 Integration   | Separate tables              | Unified                      |
| Stateful Matching       | Supported                    | Supported                    |
| Backwards Compatibility | Native iptables commands     | Can use `iptables-nft`       |

---

## Conversion from iptables to nftables

Use the `iptables-translate` tool to migrate rules:
```bash
iptables-translate -A INPUT -p tcp --dport 80 -j ACCEPT
# Output:
# nft add rule ip filter input tcp dport 80 accept
```

---

## Common Examples

### 1. Block All Traffic Except SSH
- **iptables**:
```bash
iptables -P INPUT DROP
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```
- **nftables**:
```bash
nft add table ip filter
nft add chain ip filter input { type filter hook input priority 0; policy drop; }
nft add rule ip filter input tcp dport 22 accept
```

### 2. NAT for Outgoing Traffic
- **iptables**:
```bash
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```
- **nftables**:
```bash
nft add table ip nat
nft add chain ip nat postrouting { type nat hook postrouting priority 100; }
nft add rule ip nat postrouting oifname "eth0" masquerade
```

---

## Troubleshooting Tips

- **iptables**
  - Check packet counts: `iptables -L -v -n`
  - Debug rules: `iptables -j LOG --log-prefix "IPTables-Dropped: "`

- **nftables**
  - Monitor packet hits: `nft list ruleset -a`
  - Use counters: `nft add rule ip filter input counter`

---

## Helpful Resources

- `man iptables`, `man nft`
- [nftables Wiki](https://wiki.nftables.org/)
- [iptables Documentation](https://netfilter.org/projects/iptables/index.html)
