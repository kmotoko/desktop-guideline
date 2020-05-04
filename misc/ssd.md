## SSD Optimization
+ Set BIOS/UEFI to AHCI mode for SSD.
+ Disable Hibernation
```
sudo systemctl disable hibernate.target hybrid-sleep.target suspend-then-hibernate.target
sudo systemctl mask hibernate.target hybrid-sleep.target suspend-then-hibernate.target
```
+ Reduce swappiness. Should be included in the `sysctl` config.

+ Set I/O scheduler for SSD. This should not be necessary for if the kernel is new enough (Kernel >= 4.19, Debian >= 10).

Check the current scheduler by (**replace sdX**):
`cat /sys/block/sdX/queue/scheduler`.
The scheduler currently in use will be surrounded by [ ] brackets.
The others are available schedulers.

If it is not `deadline` or `mq-deadline`, first check if the kernel knows about SSD, run:
```shell
for f in /sys/block/sd?/queue/rotational; do printf "$f is "; cat $f; done
```
It should output something like:
```shell
/sys/block/sda/queue/rotational is 1
/sys/block/sdb/queue/rotational is 1
/sys/block/sdc/queue/rotational is 0   <=== Only this is SSD!
```
**Note:** For NVMe's, the scheduler should be `none`.

+ TRIM/Discard: If the SSD is high quality and you have enough over-provisioning space, prefer not enabling it especially if you have LUKS volume as it has security implications.
