## Kali

### Updates

Full update, remove old packages, clean cache, and reboot.

```
sudo apt update && sudo apt full-upgrade -y
sudo apt autoremove -y
sudo apt clean
reboot
```

### Change between CLI and GUI

Switch to CLI:

```
sudo systemctl set-default multi-user
sudo reboot
```

Switch to GUI:

```
sudo systemctl set-default graphical
sudo reboot
```

