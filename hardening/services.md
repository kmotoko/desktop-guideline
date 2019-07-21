To list services/sockets, you can use:
```shell
sudo systemctl list-unit-files --type=service | grep enabled
sudo systemctl list-unit-files --type=socket | grep enabled
sudo systemctl list-unit-files
```
**Important:** Also disable the socket if it exists for a service.
To disable a service/socket, you can:
```
sudo systemctl stop avahi-daemon.service
sudo systemctl stop avahi-daemon.socket
sudo systemctl disable avahi-daemon.service
sudo systemctl disable avahi-daemon.socket
sudo systemctl mask avahi-daemon.service
sudo systemctl mask avahi-daemon.socket
```
Disable the following in addition to `avahi-daemon`:
```
brltty.service
pppd-dns.service
ModemManager.service
minissdpd.service
cups-browsed.service
cups.service
cups.socket
```
