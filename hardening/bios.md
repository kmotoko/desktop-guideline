## BIOS/UEFI
## General Settings
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
