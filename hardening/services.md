## Services
+ To list services/sockets, you can use:
```shell
sudo systemctl list-unit-files --type=service | grep enabled
sudo systemctl list-unit-files --type=socket | grep enabled
sudo systemctl list-unit-files
```

+ To disable a service/socket, you can:
```shell
sudo systemctl stop avahi-daemon
sudo systemctl disable avahi-daemon
sudo systemctl stop cups
sudo systemctl disable cups
sudo systemctl stop cups-browsed
sudo systemctl disable cups-browsed
```
Disable the following in addition to `avahi-daemon`:
```
brltty
pppd-dns
ModemManager
minissdpd
```
**Important:** Also disable the socket if it exists for a service.

+ Keep Tor service disabled by default:
```shell
sudo systemctl stop tor.service
sudo systemctl disable tor.service
```

+ **Teamviewer:** It self-enables after every upgrade. To prevent this behavior create `/etc/apt/apt.conf.d/88teamviewer` and add:
```
# Prevent Teamviewer self-activation after every upgrade
DPkg::Post-Invoke {"systemctl disable teamviewerd.service; systemctl stop teamviewerd.service";};

```
