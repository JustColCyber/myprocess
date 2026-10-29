# db-tools
  
## mysql
```
mysql -u <USER> -p '<PASSWORD>' -h 127.0.0.1 <HOSTANME>

mysql -u <USER> -p -h 127.0.0.1 -P 3306

SELECT current_user(), version();
SHOW DATABASES;
USE <db>; SHOW TABLES;
SHOW GRANTS;
USE <db>;
DESCRIBE _htb_staff;
SHOW CREATE TABLE _htb_staff;
SELECT COUNT(*) FROM _htb_staff;
SELECT * FROM _htb_staff LIMIT 5;



```
```
SELECT username, email, password FROM users;
```
or login and get get the users in a 1 liner:
```
mysql -uroot -p'<PASSWORD>' -h127.0.0.1 <HOSTANME> -e "SELECT username, email, password FROM users;"

```
## dbeaver
  
## mariadb
User table password hashes use Bcrypt.
```
mariadb -uroot -p'<PASSWORD>' -h127.0.0.1 <HOSTANME>
```
