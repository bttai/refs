netsh interface portproxy add v4tov4 listenport=2222 listenaddress=192.168.213.64 connectport=22 connectaddress=10.4.213.215

netsh advfirewall firewall add rule name="port_forward_ssh_2222" protocol=TCP dir=in localip=192.168.213.64 localport=2222 action=allow
netsh advfirewall show allprofiles
netsh advfirewall firewall show rule name=all 

netsh advfirewall firewall show rule name=all dir=in type=dynamic