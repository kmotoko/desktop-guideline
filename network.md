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
sudo ufw allow out 53,67,68/udp
sudo ufw allow out 25,53,80,110,443/tcp
sudo ufw allow out 143,465,587,993/tcp
sudo ufw allow out 389,901/tcp comment 'LDAP and SWAT, respectively; required for samba'
sudo ufw allow out 135,139,445/tcp comment 'EndPointMapper, NetBIOS Session, SMB over TCP; for SAMBA share'
sudo ufw allow out 137,138/udp comment 'NetBIOS stuff; for SAMBA share'
sudo ufw allow out 631/tcp comment 'Internet Printing Protocol, required for CUPS'
sudo ufw allow out 631/udp comment 'Internet Printing Protocol, required for CUPS (to send jobs)'
sudo ufw allow out 20,21/tcp comment 'FTP'
sudo ufw allow out 22 comment 'SSH'
sudo ufw allow out 989 comment 'FTP over TLS/SSH'
sudo ufw allow out 990 comment 'FTP over TLS/SSH'
sudo ufw allow out 5353/udp comment 'mDNS protocol, required for Avahi'
sudo ufw allow out to 136.243.146.209 port 1:65535 proto udp comment 'wire-desktop'
sudo ufw allow out to 136.243.148.68 port 1:65535 proto udp comment 'wire-desktop'
sudo ufw allow out to 18.194.139.224 port 1:65535 proto udp comment 'wire-desktop'
sudo ufw allow out to 54.93.169.110 port 1:65535 proto udp comment 'wire-desktop'
sudo ufw allow out to 116.203.188.16 port 1:65535 proto udp comment 'wire-desktop'
sudo ufw allow out to 116.203.137.142 port 1:65535 proto udp comment 'wire-desktop'
sudo ufw allow out to 116.203.131.31 port 1:65535 proto udp comment 'wire-desktop'
sudo ufw allow out to 77.66.84.233 port 443 comment 'dnscrypt.eu-dk'
sudo ufw allow out to 176.56.237.171 port 443 comment 'dnscrypt.eu-nl'
sudo ufw allow out to 176.103.130.130 port 5443 comment 'adguard-dns-ns1, for DNScrypt'
sudo ufw allow out 81/tcp comment 'Captive portals'
sudo ufw allow out 30303 comment 'Ethereum clients, i.e geth and wallet, listening(tcp) and node discovery(udp) port'
sudo ufw allow out 11371/tcp comment 'HKP (HTTP keyserver protocol) to reach PGP keyservers'
sudo ufw allow out 123/udp comment "Network Time Protocol (NTP)"
sudo ufw allow out 995,50002,54183/tcp comment "Electrum Bitcoin wallet"
sudo ufw allow out 6969/tcp comment 'Transmission'
sudo ufw allow out 51413/tcp comment 'TransmissionSH'
sudo ufw allow out 51413/udp comment 'Transmission'
```
