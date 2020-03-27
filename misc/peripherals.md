## Mouse
USB auto-suspend feature might not work well with certain mice. If your USB (inc. wireless USB) mouse connects but seems like occasionally disconnecting and reconnecting after it has not been moved for a while, the USB auto-suspend might be the culprit. You can either write a `udev` rule or if `laptop-mode-tools` is installed, you can also change it from there (`laptop-mode-tools` changes settings at runtime anyway).

+ Find out the device ID by: `sudo lsusb`. Output will be like:
```
Bus 002 Device 001: ID YYYY:YYYY Linux Foundation
Bus 001 Device 004: ID XXXX:XXXX Logitech
Bus 001 Device 001: ID ZZZZ:ZZZZ Linux Foundation
```
Here, `XXXX:XXXX` is the device ID for `Logitech`.

+ Edit `/etc/laptop-mode/conf.d/runtime-pm.conf`. In older versions, the module conf was called `usb-autosuspend.conf`:
```toml
# The list of Device IDs that should not use autosuspend. Use system commands or
# look into sysfs to find out the IDs of your devices.
# Example: AUTOSUSPEND_RUNTIME_DEVID_BLACKLIST="046d:c025 0123:abcd"
AUTOSUSPEND_RUNTIME_DEVID_BLACKLIST="XXXX:XXXX"
```
Note that the above rule will be effective only if `CONTROL_RUNTIME_AUTOSUSPEND=1` and `AUTOSUSPEND_USE_WHITELIST=0`.
You can also change the default idle timeout for all usb devices (default=2):
```toml
# Auto-Suspend timeout in seconds
# Number of seconds after which the USB devices should suspend
AUTOSUSPEND_TIMEOUT=5
```

+ Reload/restart `laptop-mode-tools`:
```shell
sudo systemctl reload laptop-mode.service
sudo systemctl restart laptop-mode.timer
sudo systemctl restart laptop-mode.service
```
+ Disconnect/reconnect the USB mouse.

## Mouse Wheel Speed in Gnome Wayland
Under GNOME Wayland, there is no way to set mouse wheel scroll speed, and the default is just too slow. You can change it at least for Firefox, by adding the following to `user.js`:
```javascript
user_pref("mousewheel.min_line_scroll_amount", 35);
```
