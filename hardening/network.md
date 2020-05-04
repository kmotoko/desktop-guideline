## Hosts File
Here are some hosts files to be used. Merge and de-duplicate them.
Only get the malvertising, exploit and hijacking lists from hpHosts’s.
+ https://hosts-file.net/?s=Download
+ http://someonewhocares.org/hosts/zero/
+ http://www.malwaredomainlist.com/
+ http://winhelp2002.mvps.org/hosts.htm

## Firewall
```shell
sudo ufw default deny incoming
sudo ufw default deny outgoing
sudo ufw default deny routed
sudo ufw allow out 53,67,68/udp comment 'Essential'
sudo ufw allow out 25,53,80,110,443/tcp comment 'Essential'
sudo ufw allow out 143,465,587,993/tcp comment 'Essential'
sudo ufw allow out 123/udp comment 'Network Time Protocol (NTP)'
sudo ufw allow out 443/udp comment 'DNSCrypt-proxy, non-doh, over dnscrypt'
sudo ufw allow out 81,4443,5443,8080,8081,8443/tcp comment 'Non-Standard HTTP(S)'
sudo ufw allow out 8443/udp comment 'DNSCrypt-proxy [Quad9]'
sudo ufw allow out 20,21/tcp comment 'FTP'
sudo ufw allow out 22 comment 'SSH'
sudo ufw allow out 989 comment 'FTP over TLS/SSH'
sudo ufw allow out 990 comment 'FTP over TLS/SSH'
sudo ufw allow out 389,901/tcp comment 'LDAP and SWAT [samba]'
sudo ufw allow out 135,139,445/tcp comment 'EndPointMapper, NetBIOS, SMB over TCP [samba]'
sudo ufw allow out 137,138/udp comment 'NetBIOS [samba]'
sudo ufw allow out 5353/udp comment 'mDNS protocol [Avahi]'
sudo ufw allow out 631 comment 'CUPS'
sudo ufw allow out 11371/tcp comment 'HTTP keyserver protocol'
sudo ufw allow out 30303 comment 'Ethereum clients'
sudo ufw allow out 995,50002,54183/tcp comment "Electrum Bitcoin wallet"
sudo ufw allow out 6969/tcp comment 'Transmission'
sudo ufw allow out 51413 comment 'Transmission'
sudo ufw allow out 33434:33524/udp comment 'Traceroute'
sudo systemctl stop ufw.service
sudo ufw disable
sudo systemctl start ufw.service
sudo systemctl enable ufw.service
sudo ufw enable
sudo ufw status verbose
```

To allow `ICMP` on the outgoing interface, add the following before `COMMIT` in `/etc/ufw/before.rules`. Obviously, make sure you do not have a rule that overlaps with or matches before this one:
```
# allow outbound icmp
-A ufw-before-output -p icmp -m conntrack --ctstate NEW,ESTABLISHED,RELATED -j ACCEPT
```
Many `ICMP` types for `IPv6` are already allowed in `/etc/ufw/before6.rules`, you do not need to edit it for the basic types such as echo requests, time-exceeded etc.
