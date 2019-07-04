## NVRAM Issue
If more than 50% of linux kernel is full, then `efibootmgr` will fail. This is actually due to bad UEFI manufacturers. See:
+ https://askubuntu.com/questions/1072618/could-not-prepare-boot-variable-no-space-left-on-device-grub-install-error-ef
+ https://wiki.archlinux.org/index.php/Unified_Extensible_Firmware_Interface#Requirements_for_UEFI_variable_support

To prevent it, periodically remove dump files (efi logs by grub):
In Ubuntu: /sys/fs/efi/efivars/dump-*
In Debian: /sys/firmware/efi/efivars/dump-*

## Fsck

## Smartctl
To check issues of SSD/HDD. Control and monitor storage systems using S.M.A.R.T. A value of 20 in the `wear-out level` should alert you. 100 is the best and 0 is the worst value for that attribute. You need `smartmontools` package. Source:
https://askubuntu.com/questions/325283/how-do-i-check-the-health-of-a-ssd

## BIOS Diagnostics Tools

## chkrootkit

## rkhunter
