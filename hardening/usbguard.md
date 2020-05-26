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
sudo systemctl stop usbguard.service
sudo systemctl stop usbguard-dbus.service
```

+ Take out all the **external** usb devices, turn on all the integrated devices such as wifi, bluetooth, microphone and webcam; and generate a policy of usb devices and controllers: `sudo usbguard generate-policy`. Add the output to `/etc/usbguard/rules.conf`. Make sure that all rules start with `allow`. This ensures that the internally attached devices, e.g. integrated webcam, are allowed in case they are loaded after the daemon starts.

+ Start/enable services:
```shell
sudo systemctl start usbguard.service
sudo systemctl start usbguard-dbus.service
sudo systemctl enable usbguard.service
sudo systemctl enable usbguard-dbus.service
```

+ `usbguard-applet-qt` spams the `syslog` if a particular user does not have an IPC connection permission(e.g. `usbguard-daemon: IPC connection denied`). `usbguard-applet-qt` does not run as root or as a system service, and has its autostart file in `/etc/xdg/autostart/usbguard-applet-qt.desktop`. To disable the autostart for a user without an IPC connection permission, copy it to `/home/<USERNAME>/.config/autostart/` with the same filename and add `Hidden=true` to its content:
```shell
cp /etc/xdg/autostart/usbguard-applet-qt.desktop $HOME/.config/autostart/
echo > $HOME/.config/autostart/usbguard-applet-qt.desktop
echo "Hidden=true" > $HOME/.config/autostart/usbguard-applet-qt.desktop
```
