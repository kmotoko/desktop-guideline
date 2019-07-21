After installing `usbguard` and `usbguard-applet-qt`, `/etc/usbguard/usbguard-daemon.conf` needs to be modified.
First, allow your user to allow/block devices using the qt-applet: Add your username to `IPCAllowedUsers` and `IPCAllowedGroups`.
Then, check the default config, it should be like the following:
```
ImplicitPolicyTarget=block
PresentDevicePolicy=apply-policy
PresentControllerPolicy=keep
InsertedDevicePolicy=apply-policy
RestoreControllerDeviceState=false
```
