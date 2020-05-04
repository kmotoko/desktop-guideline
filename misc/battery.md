## ACPI/Sleep
Confirm that the PC can enter deep sleep state (S3 state):
```shell
sudo dmesg | grep -i acpi | grep -i supports
cat /eys/power/state
cat /sys/power/mem_sleep
```
Outputs should look like the following, respectively:
```shell
[    0.336704] ACPI: (supports S0 S3 S4 S5)
```
```shell
freeze mem
```
```shell
s2idle [deep]
```
The important thing is to see `S3` and `s2idle [deep]`.

## Power/Battery
+ In Debian 9 and Ubuntu 16.04, several power settings are now handled by `UPower` instead of `gsettings`. Edit the values in `/etc/UPower/UPower.conf`:
```toml
UsePercentageForPolicy=true
PercentageLow=12
PercentageCritical=8
PercentageAction=5
CriticalPowerAction=PowerOff
```
Then do: `sudo systemctl restart upower.service`.
> If you read the UPower.conf file, upower used to (in v 0.99.1) only suggest using percentages in order to work around broken firmware. The default was UsePercentageForPolicy=false, which used the battery's time estimate. That makes some sense since, as a battery gets older, it lasts shorter for any given percentage. A fixed percentage that worked previously might at some point no longer allow the laptop enough time to hibernate properly. However, it looks like upower now (v 0.99.4) is defaulting to percentages and has a cryptic message about the reasoning.

+ By default, audio power saving is turned off by most drivers. It can be enabled by setting the `power_save` parameter; a time (in seconds) to go into idle mode. Check the producer and driver by `lspci -k`. If it is `intel hda`, then to idle the audio card after one second, create the following file for Intel soundcards: `/etc/modprobe.d/audio_powersave.conf` and place this line `options snd_hda_intel power_save=1`

**Note:** If using TLP for power management, audio power saving is enabled by default within TLP. So `snd_hda_intel` module option is not necessary.

## TLP
Power management tool for laptops, a better alternative to `laptop-mode-tools`.

+ If you have other management rules/tools, remove them.
+ Enable `NetworkManager-dispatcher.service`.
+ Before starting, `tlp-rdw` conflicts with `systemd-rfkill` which executes radio switching operations. In Debian 10 and Ubuntu 18.04, `tlp` deb package should already be masking both `systemd-rfkill.service` and `systemd-rfkill.socket`. Check them first.

+ Config:
```shell
sudo systemctl enable tlp.service
sudo systemctl enable tlp-sleep.service
```
In Debian 10, the `tlp` version is `v1.1`. Before `v1.3`, `tlp` configuration is different. One needs to edit `/etc/default/tlp` for `tlp < v1.3`.
```toml
TLP_ENABLE=1
MAX_LOST_WORK_SECS_ON_AC=15
MAX_LOST_WORK_SECS_ON_BAT=45
SOUND_POWER_SAVE_ON_BAT=5  # Fix clicking noises
RUNTIME_PM_DRIVER_BLACKLIST="amdgpu mei_me nouveau nvidia pcieport radeon"  # upstream (1.3) default
USB_BLACKLIST_BTUSB=1  # Bluetooth mouse etc.
USB_BLACKLIST_PHONE=1
DISK_APM_LEVEL_ON_BAT="144 144"  # Increased a bit to reduce frequent parking
DISK_IOSCHED="keep keep"  # Do not mess with the IO schedulers
SATA_LINKPWR_ON_AC="med_power_with_dipm max_performance"
SATA_LINKPWR_ON_BAT="med_power_with_dipm medium_power"
RESTORE_DEVICE_STATE_ON_STARTUP=0
DEVICES_TO_DISABLE_ON_STARTUP="bluetooth wwan"
DEVICES_TO_DISABLE_ON_BAT_NOT_IN_USE="bluetooth"
DEVICES_TO_DISABLE_ON_LAN_CONNECT="wifi wwan"
DEVICES_TO_DISABLE_ON_WIFI_CONNECT="wwan"
DEVICES_TO_DISABLE_ON_WWAN_CONNECT="wifi"
DEVICES_TO_ENABLE_ON_LAN_DISCONNECT="wifi wwan"
```
Restart the system.

+ Check: If TLP is properly working. After doing a fresh reboot, play with the PC a bit, plug and unplug the power cable, suspend and resume. Then, check:
```
sudo dmesg | grep -i tlp
sudo journalctl | grep -i tlp
sudo tlp-stat -w -v
```

+ If a usb device needs to be blacklisted you can add:
```toml
USB_BLACKLIST="XXXX:XXXX"
```
where `XXXX:XXXX` is the USB device ID which you can get via `sudo lsusb`.

**Note:** Some of the TLP features depend on `NetworkManager`, so if you have other management tool for network, you might consider totally moving to it.

> Package managers take care not to replace a user edited configuration file without explicit confirmation and create a backup copy too.
