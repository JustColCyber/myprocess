
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
curl -s -I https://example.com | grep -i strict-transport-security
```

Send your HTTP GET with a Cookie:

```
curl --cookie "1234kjasldkjf12340" http://example.com
```
