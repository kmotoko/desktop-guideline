## NX/XD Support
+ Prevents some of the buffer overflow vulnerabilities by preventing code execution on a per memory page basis.
+ Enable it from bios, it should be under one of the following names: "NX", "XD", "No Execute" or "Execute Disable".
+ Nothing should be needed to be installed into OS on 64bit modern Debian/Ubuntu's.
+ Check if it is enabled by:
`sudo dmesg | grep "Execute Disable"`
or change with other possible names mentioned above, instead of "Execute Disable".


## SSD Optimization
+ Set BIOS/UEFI to AHCI mode for SSD.
+ Disable Hibernation
```
sudo systemctl disable hibernate.target hybrid-sleep.target
sudo systemctl mask hibernate.target hybrid-sleep.target
```
+ Reduce swappiness. Should be included in the `sysctl` config.

+ Set I/O scheduler. Check the current scheduler by (**replace sdX**):
`cat /sys/block/sdX/queue/scheduler`.
The scheduler currently in use will be surrounded by [ ] brackets.

If it is not `deadline`, first check if the kernel knows about SSD, run:
```shell
for f in /sys/block/sd?/queue/rotational; do printf "$f is "; cat $f; done
```
It should output something like:
```shell
/sys/block/sda/queue/rotational is 1
/sys/block/sdb/queue/rotational is 1
/sys/block/sdc/queue/rotational is 0   <=== Only this is SSD!
```

Set the scheduler to `deadline` (**Change sd[a-z]**):
```
# set deadline scheduler for non-rotating disks
# you might wanna change sd[a-z] accordingly
ACTION=="add|change", KERNEL=="sd[a-z]", ATTR{queue/rotational}=="0", ATTR{queue/scheduler}="deadline"
```

+ TRIM/Discard: If the SSD is high quality and you have enough over-provisioning space, prefer not enabling it especially if you have LUKS volume.

## Filesystem Check
+ To force `fsck` to check the filesystem during boot, use `tune2fs`.
Check the current values:
`sudo tune2fs -l /dev/sdX`
+ Then reconfigure the maximum mount count. If the mount count is 1, it will check during every boot. Do:
`sudo tune2fs -c 2 /dev/sdX`
+ Check `/etc/fstab` parameters. The `<pass>` value should be non-zero (1 or 2). 1 is checked first then it checks 2.

## Power/Battery
+ In Debian 9 and Ubuntu 16.04, several power settings are now handled by `UPower` instead of `gsettings`. Edit the values in `/etc/UPower/UPower.conf`:
```
UsePercentageForPolicy=true
PercentageLow=12
PercentageCritical=8
PercentageAction=5
CriticalPowerAction=PowerOff
```
> If you read the UPower.conf file, upower used to (in v 0.99.1) only suggest using percentages in order to work around broken firmware. The default was UsePercentageForPolicy=false, which used the battery's time estimate. That makes some sense since, as a battery gets older, it lasts shorter for any given percentage. A fixed percentage that worked previously might at some point no longer allow the laptop enough time to hibernate properly. However, it looks like upower now (v 0.99.4) is defaulting to percentages and has a cryptic message about the reasoning.

+ By default, audio power saving is turned off by most drivers. It can be enabled by setting the `power_save` parameter; a time (in seconds) to go into idle mode. Check the producer and driver. If it is `intel hda`, then to idle the audio card after one second, create the following file for Intel soundcards: `/etc/modprobe.d/audio_powersave.conf` and place this line `options snd_hda_intel power_save=1`
