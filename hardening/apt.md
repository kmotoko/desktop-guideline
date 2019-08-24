## Apt
+ After installing `apt-transport-https`, add https to the beginning of apt sources in `/etc/apt/sources.list` and in `backports source`. Then,

```shell
sudo apt-get clean && sudo apt-get autoclean
sudo apt-get update
```
See https://wiki.debian.org/SourcesList for more info.

+ Set a cron job in `/etc/cron.daily/99apt-clean` to automatically clean the downloaded package files.
```shell
#!/bin/sh

# Clean up locally downloaded package files
# Prevent /var being filled up
apt-get clean

```
Make it executable `sudo chmod +x /etc/cron.daily/99apt-clean`.
