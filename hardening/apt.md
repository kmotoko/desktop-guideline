## Apt
+ Add https to the beginning of apt sources in `/etc/apt/sources.list` and in `backports source`. Then,

```shell
sudo apt-get clean && sudo apt-get autoclean
sudo apt-get update
```
See https://wiki.debian.org/SourcesList for more info.

**Note:** On Debian >= 10, `apt-transport-https` package is not necessary.

+ Set a cron job in `/etc/cron.daily/99apt-clean` to automatically clean the downloaded package files. Do not forget to use **absolute paths**, also it is not `bash` but `sh` shell with very limited capability.
```shell
#!/bin/sh
set -e

# Clean up locally downloaded package files
# Prevent /var being filled up
APTGET=/usr/bin/apt-get
$APTGET clean
```
Make it executable `sudo chmod +x /etc/cron.daily/99apt-clean`.
