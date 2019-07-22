Change the kernel parameters at runtime. **Note:** From version 207 and 21x, systemd only applies settings from `/etc/sysctl.d/*.conf`. If you had customized `/etc/sysctl.conf`, you need to rename it as `/etc/sysctl.d/99-sysctl.conf`. If you had e.g. `/etc/sysctl.d/foo`, you need to rename it to `/etc/sysctl.d/foo.conf`.
Edit the appropriate file to include the following:

```
###
### GENERAL SYSTEM SECURITY OPTIONS ###
###

# Restart after X seconds in case of kernel panic
kernel.panic = 300

# Enable ExecShield protection
kernel.randomize_va_space = 2

# Restrict core dumps
fs.suid_dumpable = 0

# Hide exposed kernel pointers
kernel.kptr_restrict = 1

# Restrict access to kernel logs
kernel.dmesg_restrict = 1

# Restrict ptrace scope
# 1=only parent process can be debugged
kernel.yama.ptrace_scope = 1

###
### Deprecated/Not-in-use keys for security
###

# The contents of /proc/<pid>/maps and smaps files are only visible to
# readers that are allowed to ptrace() the process
# kernel.maps_protect = 1

# Enable ExecShield protection
# kernel.exec-shield = 1

###
### IMPROVE SYSTEM MEMORY MANAGEMENT ###
###

# Do less swapping
vm.swappiness = 3

# Keep at least 128MB of free RAM space available
vm.min_free_kbytes = 131072

###
### GENERAL NETWORK SECURITY OPTIONS ###
###

# Keep BPF JIT compiler disabled
net.core.bpf_jit_enable = 0

#Prevent SYN attack, enable SYNcookies (they will kick-in when the max_syn_backlog reached)
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_syn_retries = 2
net.ipv4.tcp_synack_retries = 2
net.ipv4.tcp_max_syn_backlog = 4096

# Disables packet forwarding
net.ipv4.ip_forward = 0
net.ipv4.conf.all.forwarding = 0
net.ipv4.conf.default.forwarding = 0
net.ipv6.conf.all.forwarding = 0
net.ipv6.conf.default.forwarding = 0

# Disables IP source routing
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0
net.ipv6.conf.all.accept_source_route = 0
net.ipv6.conf.default.accept_source_route = 0

# Enable IP spoofing protection, turn on source route verification
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1

# Disable ICMP Redirect Acceptance
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0
net.ipv6.conf.all.accept_redirects = 0
net.ipv6.conf.default.accept_redirects = 0

# Don't relay bootp
net.ipv4.conf.all.bootp_relay = 0

# Don't proxy arp for anyone
net.ipv4.conf.all.proxy_arp = 0

# Modes for sending replies in response to received ARP requests that resolve local target IP addresses
# Mode '1': reply only if the target IP address is local address configured on the incoming interface
net.ipv4.conf.all.arp_ignore = 1

# Restriction levels for announcing the local source IP address from IP packets in ARP requests sent on interface
# Mode '2': ignore the source address in the IP packet and try to select local address that we prefer for talks with the target host
net.ipv4.conf.all.arp_announce = 2

# Don't ignore directed pings
# Useful for monitoring tools etc.
# Might want to limit requests/s in iptables
net.ipv4.icmp_echo_ignore_all = 0

# Enable ignoring broadcasts request
net.ipv4.icmp_echo_ignore_broadcasts = 1

# Enable bad error message Protection
net.ipv4.icmp_ignore_bogus_error_responses = 1

# ICMP ratelimit
# Affected ICMP types defines in icmp_ratemask
net.ipv4.icmp_ratelimit = 100

# ICMP ratemask
net.ipv4.icmp_ratemask = 88089

# Enable a fix for RFC1337 - time-wait assassination hazards in TCP
net.ipv4.tcp_rfc1337 = 1

###
### TUNING NETWORK PERFORMANCE ###
###

# Limit the maximum memory used to reassemble IP fragments (CVE-2018-5391)
net.ipv4.ipfrag_low_thresh = 196608
net.ipv6.ip6frag_low_thresh = 196608
net.ipv4.ipfrag_high_thresh = 262144
net.ipv6.ip6frag_high_thresh = 262144

# This will ensure that immediately subsequent connections use the new values
# ALWAYS COMES LAST
net.ipv4.route.flush = 1
net.ipv6.route.flush = 1
```

Before appyling the settings, disable `apport` automatic crash report generation (at least in Ubuntu 18.04).
This service overrides `fs.suid_dumpable` value. Thus do:
```
sudo systemctl stop apport.service
sudo systemctl disable apport.service
sudo systemctl mask apport.service
```
To be double sure, also change the `enabled` value to `0` in `/etc/default/apport`.

Use `sudo sysctl --system` to apply settings. **Important:** Check and make sure if the new config sticks after reboot (`sudo sysctl --all`).

**Note**: `kernel.exec-shield` and `kernel.maps_protect` keys are extremely important. However, `kernel.maps_protect` became non-optional in kernel 2.6.27 and `kernel.exec-shield` exists in RedHat/CentOS, currently not in Debain 9 at least. But you can still do `kernel.randomize_va_space = 2` for Debian/Ubuntu. Do not change `vm.mmap_min_addr` without extreme care as it has security implications, as long as you are using a fairly modern kernel (includes Debian 9, Ubuntu 16.04 and newer) the default should be fine. Also, `fs.file-max` should be fine with the defaults.
