## Virtualization
+ First enable `Virtualization Support` in BIOS. It may go by names `VT-x` (intel) or `VT-d` (amd).
+ Check hardware support: `lscpu | grep -i virt`
+ Check if the `kvm` (Kernel Virtual Machine for faster processing) kernel module is loaded: `lsmod | grep -i kvm`
