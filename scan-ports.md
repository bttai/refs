## netcat
```sh

# TCP
nc -nvv -z -w 1 $RHOST p1-pn
# UDP
nc -nvv -z -w 1 -u $RHOST p1-pn

```

## Nmap

```sh

nmap -p- $RHOST
nmap -sV -sC -p 22,445,139 $RHOST
sudo nmap -sU -p- $RHOST
nmap -sn $IP
nmap -n -Pn -sT $RHOST
nmap -sT -A --top-ports=200 $RHOST -oG top-port-sweep.txt

```

## Powershell

```powershell
Test-NetConnection -Port $RPORT $RHOST

1..1024 | % {echo ((New-Object Net.Sockets.TcpClient).Connect("192.168.50.151", $_)) "TCP port $_ is open"} 2>$null

```

## Telnet

```sh

telnet $RHOST

```
