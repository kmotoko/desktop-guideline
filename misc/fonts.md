## Fontconfig
Custom `fontconfig` rules should be placed in `/etc/fonts/local.conf`:

```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <match target="font">
    <edit name="embeddedbitmap" mode="assign">
      <bool>false</bool>
    </edit>
    <edit name="antialias" mode="assign">
      <bool>true</bool>
    </edit>
    <edit name="hinting" mode="assign">
      <bool>true</bool>
    </edit>
    <edit name="autohint" mode="assign">
      <bool>false</bool>
    </edit>
    <edit name="hintstyle" mode="assign">
      <const>hintslight</const>
    </edit>
    <edit name="rgba" mode="assign">
      <const>rgb</const>
    </edit>
    <edit name="lcdfilter" mode="assign">
      <const>lcdlight</const>
    </edit>
  </match>
</fontconfig>
```
Then do `sudo fc-cache -r -v`.
Check if your config is being applied via: `fc-conflist | grep -i local`

## Xresources
Some software does not read fontconfig, for those you need Xresources.
Add to `~/.Xresources`:
```yaml
Xft.autohint: 0
Xft.hinting: 1
Xft.hintstyle: hintslight
Xft.antialias: 1
Xft.rgba: rgb
Xft.lcdfilter: lcdlight
```
Apply by `xrdb -merge ~/.Xresources`.
Check by `xrdb -q`.
@TODO: Currently, this does not auto-load with Wayland session in Gnome.
