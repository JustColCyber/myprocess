## 2049/tcp NFS 

check what shares are exported:

```
showmount -e 10.1.1.1
```
Output looks like:
```
Export list for 10.1.1.1:
/srv/nfs/directory *
```

Mount it to a temporary directory:

```
mkdir /tmp/local-system-dir
sudo mount -t nfs 10.1.1.1:/srv/nfs/directory /tmp/local-system-dir -o nolock
```

Grab the loot:
```
ls -laR /tmp/local-system-dir 
cp /tmp/local-system-dir/file1.txt /home/user1/Downloads/file1.txt 
```