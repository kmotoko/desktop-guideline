## Apt
After installing `apt-transport-https`, add https to the beginning of apt sources in `/etc/apt/sources.list` and in `backports source`. Then,

```shell
sudo apt-get clean && sudo apt-get autoclean
sudo apt-get update
```
See https://wiki.debian.org/SourcesList for more info.
