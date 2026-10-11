## Kali

### Updates

Full update, remove old packages, clean cache, and reboot.

```
sudo apt update && sudo apt full-upgrade -y
sudo apt autoremove -y
sudo apt clean
reboot
```