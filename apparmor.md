## Apparmor
+ If using Debian <= 9 (enabled by default in Debian >= 10):
```shell
sudo apt-get install apparmor apparmor-profiles apparmor-profiles-extra
sudo mkdir -p /etc/default/grub.d
echo 'GRUB_CMDLINE_LINUX_DEFAULT="$GRUB_CMDLINE_LINUX_DEFAULT apparmor=1 security=apparmor"' | sudo tee /etc/default/grub.d/apparmor.cfg
sudo update-grub
sudo reboot
```
+ Check by: `sudo aa-status`.

**Important:** Even if using Debian >= 10, install the profiles and utils:
`sudo apt-get install apparmor-profiles apparmor-profiles-extra apparmor-utils`
