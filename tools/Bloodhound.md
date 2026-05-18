REFS:

https://github.com/SpecterOps/BloodHound

Sharphound downloads: https://github.com/SpecterOps/BloodHound-Legacy/tree/master/Collectors

## Setup

```
sudo apt install bloodhound
bloodhound-start
```

### Neo4j password changes

 [i] You need to change the default password for neo4j
     Default credentials:
         user: neo4j
         password: neo4j
[!] IMPORTANT: Once you have setup the new password, please update /etc/bhapi/bhapi.json with the new password before running bloodhound


### Neo4j password reset

sudo nano /etc/neo4j/neo4j.conf

### Bloodhound admin password

Reset Bloodhound admin password:

```
sudo env bhe_recreate_default_admin=true bloodhound
```

./bloodhound-cli resetpwd

## Sharphound

```
./SharpHound.exe
```

Copy the <randomString>_BloodHound.zip

## Bloodhound

Search or Explore for objects and their relationships.