## GNOME Greeting Screen
+ From GNOME docs:
> When choosing an appropriate picture for the logo to your login screen, consider the following picture requirements:
+ All the major formats are supported: ANI, BPM, GIF, ICNS, ICO, JPEG, JPEG 2000, PCX, PNM, PBM, PGM, PPM, GTIFF, RAS, TGA, TIFF, XBM, WBMP, XPM, and SVG.
+ The size of the picture scales proportionally to the height of 48 pixels. So, if you set the logo to 1920x1080, for example, it changes into an 85x48 thumbnail of the original picture.

+ Copy the splash screen:
`sudo cp <SPLASH_IMAGE.svg> /usr/share/pixmaps/`

+ If it does not exist, create a gdm profile in `/etc/dconf/profile/gdm`:
```
user-db:user
system-db:gdm
file-db:/usr/share/gdm/greeter-dconf-defaults
```

+ Create the file `/etc/dconf/db/gdm.d/01-banner` and include:
```
[org/gnome/login-screen]
# Show a banner message
banner-message-enable=true
banner-message-text="<MESSAGE>"
logo='/usr/share/pixmaps/<SPLASH_IMAGE.svg>'
```

+ Create the file `/etc/dconf/db/gdm.d/00-login-screen` and include:
```
[org/gnome/login-screen]
# Do not show the user list
disable-user-list=true
```

+ Update dconf db: `sudo dconf update`.

References:
+ https://help.gnome.org/admin/system-admin-guide/stable/login-logo.html.en
+ https://help.gnome.org/admin/system-admin-guide/stable/login-banner.html.en
+ https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/desktop_migration_and_administration_guide/customizing-login-screen
