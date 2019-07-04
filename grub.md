## GRUB Splash Screen
+ Check how GRUB recognizes the display.
Press `c` while in GRUB. Type `vbeinfo` or `videoinfo` if the first one does not work. It will list the supported resolutions and color depths. At the bottom, you should see the preferred mode. This will be your resolution for the later steps. Sometimes due to buggy drivers, this value might not be a sane value, if that is the case use the closest value for your display from the list.

+ GRUB 2 can use PNG, JPG/JPEG and TGA images for the background. The image should be:
>
+ JPG/JPEG images must be 8-bit (256 color). Else you will get errors saying "Too many Huffman tables". Since most of the time you will not want to limit yourself to 256 colors (which is totally yesteryear) you will probably find PNG much preferable.
+ Images should be non-indexed, RGB.
+ The GIMP image editor is one application which can edit images to conform to the GRUB 2 standards. Use the Image > Mode menu options to set the properties to RGB and ensure the mode is not set to Indexed.

+ Copy the custom splash image:
`sudo cp <SPLASH_IMAGE.png> /boot/grub/`

+ Edit `/etc/default/grub` to include:
```
GRUB_GFXMODE=WIDTHxHEIGHTxCOLORDEPTH
# Example: GRUB_GFXMODE=1920x1080x32
GRUB_BACKGROUND="/boot/grub/SPLASH_IMAGE.png"
```
Also make sure that `GRUB_CMDLINE_LINUX_DEFAULT` includes `splash` as a parameter.

+ Run `sudo update-grub`.

References:
+ https://askubuntu.com/questions/54067/how-do-i-safely-change-grub2-screen-resolution
+ https://wiki.debian.org/plymouth
+ https://help.ubuntu.com/community/Grub2/Displays
+ https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/desktop_migration_and_administration_guide/customizing-login-screen
