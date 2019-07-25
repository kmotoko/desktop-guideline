## Partition Scheme
There are important partition mount options:
`nodev`: disable block devices on the filesystem
`nosuid`: ignore set-user-identifier or set-group-identifier bits
`noexec`: disable execution of scripts/binaries

+ If a partition is pure data use all of the `nodev`, `nosuid`, and `noexec`.
+ Harden world-writable directories: `/tmp`, `/var/tmp`, `/dev/shm`.
+ Some programs, such as `dpkg` and `debconf`, use `/tmp` to execute scripts. See below to fix the issue.
+ There is no practical way to make the whole `/var` `noexec` since some programs place executable scripts there. Instead, separate partitions for `/var/tmp` and `/var/log`.
+ Some resources suggest making `/usr` read-only. IMHO, this does not add much, as it is required to have root privileges to write there. And if someone has compromised root, he/she can do anything already. The real benefit of mounting it read-only would be in cases where you run unsafe code as root, but again you should already never do that.
+ `/home` can be considered for `noexec`, but not that convenient for dev PCs. However, you can use `nosuid` and `nodev`.
+ `/dev/shm` or `/run/shm` are virtual filesystems that are used for shared memory. They are world-writable. Depending on the distro, it is one of them. For ecample, in Debian 9, `/run/shm` is a symbolic link to `/dev/shm`, therefore `/dev/shm` is the one that needs a change in mount options. Before doing anything, check which one exists/is symbolic link, and also check the output of `mount | grep -i shm` to see which one is actually mounted.

That was it for the basic understanding. Now is the time for the actual config.

+ Strict options for `/etc/fstab`: The should look like the following (disregard everything except the mount options and mount points).
```
/dev/sdaX   /var  ext4    defaults,nodev 0      2
/dev/sdaY   /var/tmp  ext4    defaults,nodev,nosuid    0       2
/dev/sdaZ  /var/log ext4    defaults,nodev,nosuid,noexec    0       2
/dev/sdaB   /tmp  ext4    defaults,nodev,nosuid    0       2
/dev/sdaD  /home  ext4    defaults,nosuid,nodev                0       2
```
For `/`, `/boot`, `/boot/efi` (or `/efi`), go with the defaults.

+ More strict options for `/etc/fstab`: The should look like the following (disregard everything except the mount options and mount points). **Note:** This might cause various issues with GUI programs. Instead of doing this config, you can noexec specific programs with firejail.
```
/dev/sdaX   /var  ext4    defaults,nodev 0      2
/dev/sdaY   /var/tmp  ext4    defaults,nodev,nosuid,noexec    0       2
/dev/sdaZ  /var/log ext4    defaults,nodev,nosuid,noexec    0       2
/dev/sdaB   /tmp  ext4    defaults,nodev,nosuid,noexec    0       2
/dev/sdaD  /home  ext4    defaults,nosuid,nodev                0       2
```
For `/`, `/boot`, `/boot/efi` (or `/efi`), go with the defaults.

As said earlier, `dpkg` needs `/tmp` to be executable. A workaround would be to mount `/tmp` with `exec` before `dpkg` and then remount with `noexec` after it is done. Create `/etc/apt/apt.conf.d/99tmpfix` and place:
```
DPkg::Pre-Invoke{"mount -o remount,exec /tmp";};
DPkg::Post-Invoke {"mount -o remount /tmp";};
```
If you want to manually run `update-initramfs -u`, again you need to remount `/tmp` with `exec` as shown before.

+ Secure shared memory. Once you have determined the shm dir (`/run/shm` or `/dev/shm`) add the following to the `/etc/fstab` (replace the virtual mount point with the appropriate path):
```
none /run/shm tmpfs rw,noexec,nosuid,nodev 0 0
```
You could also mount it read-only by `none /run/shm tmpfs defaults,ro 0 0`, but this might cause issues with several software, including Chromium. Also, as the `man page` says above, you could also use `tmpfs` instead of `none`.

+ Reboot and check mount flags with: `mount`

## Disable Unused Filesystems
Create `/etc/modprobe.d/dev-sec.conf` file with the following contents:
```
install cramfs /bin/true
install freevxfs /bin/true
install jffs2 /bin/true
```
Do not disable `vfat` as it is necessary for EFI.
Do not disable `squashfs` as it is necessary for `snap`.

To list all available filesystems: `ls -l /lib/modules/$(uname -r)/kernel/fs`

## Permissions
**Important:** Never do `chmod` recursively for home dir!
+ For existing users: Set owner of the `$HOME` to you and remove the world-readable bit:
```shell
chown myuser:myuser $HOME
chmod 750 $HOME
```
+ For users to be created: By default, home directory is created with permission `755`. Change `DIR_MODE` in `/etc/adduser.conf`:
```
DIR_MODE=0750
```

+ In addition to `adduser` command, there is an older (and discouraged) one, `useradd`. Change the `UMASK` value there to prevent accidental bypasses. In `/etc/login.defs`, change `UMASK` to: `UMASK		027` which means a permission of `750`.

## Filesystem Check
+ To force `fsck` to check the filesystem during boot, use `tune2fs`.
Check the current values:
`sudo tune2fs -l /dev/sdX`
+ Then reconfigure the maximum mount count. If the mount count is 1, it will check during every boot. Do:
`sudo tune2fs -c 2 /dev/sdX`
+ Check `/etc/fstab` parameters. The `<pass>` value should be non-zero (1 or 2). 1 is checked first then it checks 2.
