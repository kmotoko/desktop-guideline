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

+ By default, audio power saving is turned off by most drivers. It can be enabled by setting the `power_save` parameter; a time (in seconds) to go into idle mode. Check the producer and driver by `lspci -k`. If it is `intel hda`, then to idle the audio card after one second, create the following file for Intel soundcards: `/etc/modprobe.d/audio_powersave.conf` and place this line `options snd_hda_intel power_save=1`
