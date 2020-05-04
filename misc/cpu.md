## CPU Microcode
After installing the microcode for your CPU, restart the system and check if the microcode updates are applied:
```shell
dmesg | grep -i microcode
# OR
journalctl -b -k | grep -i microcode
```
Then you would see something like:
```shell
[    0.000000] microcode: microcode updated early to revision 0xd6, date = 2019-10-03
[    2.239466] microcode: sig=0x406e3, pf=0x80, revision=0xd6
[    2.239510] microcode: Microcode Update Driver: v2.2.
```
Depending on the available patch/BIOS version, you might not see the line `microcode: microcode updated early to revision 0xd6, date = 2019-10-03`.
