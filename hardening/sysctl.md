Change the kernel parameters at runtime. **Note:** From version 207 and 21x, systemd only applies settings from `/etc/sysctl.d/*.conf`. If you had customized `/etc/sysctl.conf`, you need to rename it as `/etc/sysctl.d/99-sysctl.conf`. If you had e.g. `/etc/sysctl.d/foo`, you need to rename it to `/etc/sysctl.d/foo.conf`.
Edit the appropriate file to include the following:

```toml
###
### GENERAL SYSTEM SECURITY OPTIONS ###
###

# Restart after X seconds in case of kernel panic
kernel.panic = 90

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

# Protect links on the filesystem
fs.protected_hardlinks = 1
fs.protected_symlinks = 1

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

# Consensus: 10% for 1GB = 100MB, for 16GB 500MB is sane
# Once you set dirty_ratio, a 1:2 ratio between
# dirty_ratio : dirty_background_ratio is reasonable
# Note that higher ratio values may increase performance
# but it also increases the risk of data loss
vm.dirty_ratio = 4
vm.dirty_background_ratio = 2

# Rule for minimum free KB of RAM: (installed_mem / num_of_cores) * 0.06
vm.min_free_kbytes = 251658

# On Debian 10, the default = 100
# Decreased values might increase responsiveness
# NEVER EVER set it to 0
vm.vfs_cache_pressure = 80


###
### GENERAL NETWORK SECURITY OPTIONS ###
###

# Do not allow unprivileged users to run code in the kernel through BPF
kernel.unprivileged_bpf_disabled = 1

# Previously, it was recommended to disable the BPF JIT compiler
# Some Spectre variants make use BPF interpreter
# Thus, in newer kernels, BPF JIT compiler is always ON
# if you set it to ON, also use hardening
# bpf_jit_harden = 1 means harden the unprivileged code
# Full hardening (value=2) might cripple some tracing/debugging functions
net.core.bpf_jit_enable = 1
net.core.bpf_jit_harden = 1

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

# Turn on the tcp_timestamps, accurate timestamp make TCP congestion control algorithms work better
# Do not turn it off, it has security implications
net.ipv4.tcp_timestamps = 1

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

# Disable logging martian packages
# Otherwise it might cause DOS
net.ipv4.conf.default.log_martians = 0
net.ipv4.conf.all.log_martians = 0

###
### TUNING NETWORK PERFORMANCE ###
###

# Avoid falling back to slow start after a connection goes idle
# keeps our cwnd large with the keep alive connections (kernel > 3.6)
# When enabled, kills persistent single connection performance
net.ipv4.tcp_slow_start_after_idle = 0

# Enable MTU probing
net.ipv4.tcp_mtu_probing = 1

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

Use `sudo sysctl --system` to apply settings, and then restart. **Important:** Check and make sure if the new config sticks after reboot (`sudo sysctl --all`).

**Note**: If the `laptop-mode-tools` is installed, it overrides `vm.dirty_ratio` and `vm.dirty_background_ratio`. This **does not** happen with `tlp`. Thus, if `laptop-mode-tools` is being used, edit the following lines in `/etc/laptop-mode/laptop-mode.conf`:
```
# Dirty synchronous ratio.  At this percentage of dirty pages the process
LM_DIRTY_RATIO=<value_1>
NOLM_DIRTY_RATIO=<value_1>

# Allowed dirty background ratio, in percent.  Once DIRTY_RATIO has been
# of dirty memory to dirty_background_ratio.  Set this nice and low, so once
LM_DIRTY_BACKGROUND_RATIO=<value_2>
NOLM_DIRTY_BACKGROUND_RATIO=<value_2>

```

**Note**: `kernel.exec-shield` and `kernel.maps_protect` keys are extremely important. However, `kernel.maps_protect` became non-optional in kernel 2.6.27 and `kernel.exec-shield` exists in RedHat/CentOS, currently not in Debain 9 at least. But you can still do `kernel.randomize_va_space = 2` for Debian/Ubuntu. Do not change `vm.mmap_min_addr` without extreme care as it has security implications, as long as you are using a fairly modern kernel (includes Debian 9, Ubuntu 16.04 and newer) the default should be fine. Also, `fs.file-max` should be fine with the defaults.
