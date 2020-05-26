## Sensors
+ Use `lm-sensors` to probe the hardware sensors.
+ First detect the required kernel modules:
```shell
sudo sensors-detect
```
+ The safe choice is to go with the defaults (just press Enter).
+ In the last question, it will ask to write the detected kernel module(s) into `/etc/modules`, accept it.
+ Reboot.
+ Confirm that the module(s) is loaded by:
```shell
lsmod | grep -i <module_name>
```
