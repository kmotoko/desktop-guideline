## BIOS/UEFI
### General Settings
+ Password-protect the BIOS setup screen
+ Check if there is a separate option to lock down the boot order. If so, make sure that the default boot order first tries the OS bootloader, not the external ports, and then lock the settings.
+ Enable Secure Boot.
+ Enable the hardware security features provided by BIOS (Memory Protection etc.).

### NX/XD Support
+ Prevents some of the buffer overflow vulnerabilities by preventing code execution on a per memory page basis.
+ Enable it from bios, it should be under one of the following names: "NX", "XD", "No Execute" or "Execute Disable".
+ Nothing should be needed to be installed into OS on 64bit modern Debian/Ubuntu's.
+ Check if it is enabled by:
`sudo dmesg | grep "Execute Disable"`
or change with other possible names mentioned above, instead of "Execute Disable".

### Secure Boot
+ If you don't want to install your own keys, but unsure if the computer has the Microsoft keys/certs in the DB, you can check the installed keys in BIOS via `efi-readvar` command of the `efitools` package.
+ If there is, enable the Secure Boot and reboot.
+ Check with `bootctl status`.

### BIOS Update
#### Thinkpad
On Thinkpads, `geteltorito` tool from `genisoimage` (in Debian/Ubuntu) works well to extract the boot image from the Lenovo-provided iso image. One catch is that in order to be able to boot from the USB, you need to temporarily disable the `Boot Order Lock` and `Secure Boot` options, and  temporarily enable the `CSM Support` option while in the `UEFI Only Mode`. Then it is as easy as:
```shell
geteltorito -o <image>.img <image>.iso
sudo umount <device_mountpoint>
sudo dd if=<image>.img of=<device> && sync
```
