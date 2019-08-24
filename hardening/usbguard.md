## USBguard

After installing `usbguard` and `usbguard-applet-qt`, `/etc/usbguard/usbguard-daemon.conf` needs to be modified.

+ First, allow your user to allow/block devices using the qt-applet: Add your username to `IPCAllowedUsers` and `IPCAllowedGroups`.

+ Then, check the default config, it should be like the following:
```
ImplicitPolicyTarget=block
PresentDevicePolicy=apply-policy
PresentControllerPolicy=keep
InsertedDevicePolicy=apply-policy
RestoreControllerDeviceState=false
```

+ Stop the services to make sure nothing is blocked before the policy generation:
```shell
sudo systemctl start usbguard.service
sudo systemctl start usbguard-dbus.service
```

+ Take out all the **external** usb devices, turn on all the integrated devices such as wifi, bluetooth, microphone and webcam; and generate a policy of usb devices and controllers: `sudo usbguard generate-policy`. Add the output to `/etc/usbguard/rules.conf`. Make sure that all rules start with `allow`. This ensures that the internally attached devices, e.g. integrated webcam, are allowed in case they are loaded after the daemon starts.

+ Start/enable services:
```shell
sudo systemctl start usbguard.service
sudo systemctl start usbguard-dbus.service
sudo systemctl enable usbguard.service
sudo systemctl enable usbguard-dbus.service
```
