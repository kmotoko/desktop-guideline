+ Make sure nothing is using port 53: `sudo ss -lp 'sport = :domain'`

+ Edit `server_names` in `/etc/dnscrypt-proxy/dnscrypt-proxy.toml`:
```
server_names = ['quad9-dnscrypt-ip4-filter-pri', 'quad9-dnscrypt-ip4-filter-alt']
# server_names = ['dnscrypt.eu-dk', 'dnscrypt.eu-dk-ipv6']
require_dnssec = true
require_nolog = true
dnscrypt_servers = true
doh_servers = true
```
+ `sudo cp /etc/resolv.conf /etc/resolv.conf.backup`

+ `sudo rm -f /etc/resolv.conf`, in case it is a symlink.

+ Create and edit `/etc/resolv.conf` to have:
```
nameserver 127.0.2.1
options edns0 single-request
```
`edns0` is required for DNSSEC usage, `single-request` sequentially sends IPv4 and IPv6 requests instead of sending in parallel. The latter might be required under some circumstances.

+ Lock `resolv.conf`: `sudo chattr +i /etc/resolv.conf`

+ Start/Enable:
```shell
sudo systemctl start dnscrypt-proxy
sudo systemctl restart dnscrypt-proxy
sudo systemctl enable dnscrypt-proxy
```
+ Test EDNS0: `drill rs.dns-oarc.net TXT`. Look for "EDNS: version: 0".
