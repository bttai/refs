
# LDAP & ldapsearch Cheat Sheet

## **Basic Concepts**
- **LDAP**: Lightweight Directory Access Protocol, used to access and manage directory information over a network.
- **DN (Distinguished Name)**: Identifies an entry in the directory, e.g., `cn=admin,dc=example,dc=com`.
- **Base DN**: Starting point for searches, e.g., `dc=example,dc=com`.
- **Attributes**: Information fields, e.g., `cn` (Common Name), `uid` (User ID).

---

## **Common ldapsearch Syntax**
General format:
```bash
ldapsearch [options] -H <URI> -b <BaseDN> <Filter> [Attributes]
```

### **Examples**
1. **Retrieve All Entries**:
   ```bash
   ldapsearch -x -H ldap://<IP> -b "dc=example,dc=com" "(objectClass=*)"
   ```

2. **List Users**:
   ```bash
   ldapsearch -x -H ldap://<IP> -b "dc=example,dc=com" "(objectClass=person)"
   ```

3. **List Groups**:
   ```bash
   ldapsearch -x -H ldap://<IP> -b "dc=example,dc=com" "(objectClass=group)"
   ```

4. **Search by CN**:
   ```bash
   ldapsearch -x -H ldap://<IP> -b "dc=example,dc=com" "(cn=admin)"
   ```

5. **Authenticate with Credentials**:
   ```bash
   ldapsearch -x -H ldap://<IP> -D "cn=admin,dc=example,dc=com" -w "<password>" -b "dc=example,dc=com"
   ```

6. **Query Specific Attributes**:
   ```bash
   ldapsearch -x -H ldap://<IP> -b "dc=example,dc=com" "(objectClass=person)" cn mail
   ```

7. **Root DSE Query** (Server Info):
   ```bash
   ldapsearch -x -H ldap://<IP> -b "" -s base "(objectClass=*)"
   ```

8. **Secure LDAP (LDAPS)**:
   ```bash
   ldapsearch -x -H ldaps://<IP> -b "dc=example,dc=com"
   ```
---

## **Common Filters**
- `(objectClass=*)`: Match all objects.
- `(cn=admin)`: Match entries with `cn` = admin.
- `(uid=*)`: Match all user IDs.
- `(&(objectClass=person)(uid=*))`: Match all persons with a UID.
- `(memberOf=cn=admins,dc=example,dc=com)`: Match entries in the `admins` group.

---

## **Authentication Options**
- `-D`: Specify the bind DN (e.g., `cn=admin,dc=example,dc=com`).
- `-w`: Password for the bind DN.
- `-x`: Simple authentication (no SASL).

---

## **LDAP Tools**
- **ldapsearch**: Query the LDAP server.
- **ldapadd**: Add new entries to the directory.
- **ldapdelete**: Delete entries.
- **ldappasswd**: Change passwords.

---

## **Troubleshooting**
- **Error: Invalid DN Syntax (34)**:
  - Check DN formatting, e.g., `cn=admin,dc=example,dc=com`.
- **Error: Can't Contact LDAP Server (-1)**:
  - Verify server IP/port and connectivity.
- **Result: 32 No Such Object**:
  - Check Base DN or search scope.

---

## **Useful Tools**
- `nmap` for LDAP Enumeration:
  ```bash
  nmap -p 389 --script ldap-rootdse <IP>
  ```
- `Hydra` for LDAP Brute-Force:
  ```bash
  hydra -L users.txt -P passwords.txt ldap://<IP>
  ```
