## GPU
On Debian 10, if you chose to install/enable `intel-media-va-driver` over `i965-va-driver`, you need to set an environment variable for it to be active. Add the following to `/etc/environment`:
```toml
LIBVA_DRIVER_NAME=iHD
```
Restart the computer and confirm it via `echo $LIBVA_DRIVER_NAME` and then do `vainfo`, you should see `Intel iHD` driver instead of `i915` or `i965`.
