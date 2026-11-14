## mysql

Login
```
mysql -u glpi -p'8rhu0L6Pw4Y7' -h 127.0.0.1 glpi
```
Not login but read stuff?
```
mysql -u glpi -p'8rhu0L6Pw4Y7' -h 127.0.0.1 -e "SHOW DATABASES;"

```
### See DBs
```
mysql -u glpi -p'8rhu0L6Pw4Y7' -h 127.0.0.1 -e "SHOW DATABASES;"
```
### REad users

SELECT id, name, realname, firstname, password, is_active, is_deleted FROM glpi_users;

### Search Plain Text Leaks
```
SELECT id, name, content FROM glpi_tickets LIMIT 20;
SELECT * FROM glpi_notepads LIMIT 20;
```
