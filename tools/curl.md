
Ignore a self signed cert:

```
curl -k https://example.com
```

Show the reddirect, TLS, cert stuff, and everything:
```
curl -v -L https://example.com
```

Show me just the headers:
```
curl -I https://example.com
```

Show me the HSTS goodness:
```
curl -s -I https://owasp.org | grep -i strict-transport-security
```
Request a PHP session ID / cookie:

```
curl -c cookies.txt -d "username=<YOUR_USERNAME>&password=<YOUR_PASSWORD>" -X POST http://example/index.php
```

Send your HTTP GET with a Cookie:

```
curl --cookie "1234kjasldkjf12340" http://example.com
```
Force the use an HTTP GET request:
```
curl -G "https://api.example.com/search" -d "query=curl command" -d "page=1"
```

Upload a file:

```
curl -k -i \
  -b "PHPSESSID=<your_session_cookie>" \
  -F "blob1=@exploit.zip;type=application/zip" \
  -F "op=save" \
  -F "id_module=14" \
  -F "id_plugin=48" \
  https://example.com/actions.php
```