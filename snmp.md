## nmap


```sh
# to scan for open SNMP ports
sudo nmap -sU --open -p 161 192.168.50.1-254

onesixtyone -c community.txt -i targets.txt

# to enumerates the entire MIB tree 
$ snmpwalk -c public -v1 -t $RHOST

# to enumerates the Windows users on the machine.
# -v 1|2c|3
$ snmpwalk -c public -v1 $RHOST 1.3.6.1.4.1.77.1.2.25

# to enumerate all the currently running processes:
$ snmpwalk -c public -v1 $RHOST 1.3.6.1.2.1.25.4.2.1.2

# query all the software that is installed on the machine
$ snmpwalk -c public -v1 $RHOST 1.3.6.1.2.1.25.6.3.1.2

# to list all the current TCP listening ports:
$ snmpwalk -c public -v1 $RHOST 1.3.6.1.2.1.6.13.1.3

# download-mibs
apt-get install snmp-mibs-downloader
# Finally comment the line saying "mibs :" in /etc/snmp/snmp.conf

# to enumerate more about the system
# -v 1|2c|3
# communities : public, private, manager

snmpwalk -v X -c $COM $RHOST 
snmpwalk -v X -c $COM $RHOST NET-SNMP-EXTEND-MIB::nsExtendOutputFull
snmpwalk -v X -c $COM $RHOST NET-SNMP-EXTEND-MIB::nsExtendObjects #get extended

nmap --script "snmp* and not snmp-brute" $RHOST

```