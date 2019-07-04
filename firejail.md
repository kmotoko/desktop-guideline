## Using with Apparmor
Upon installation, firejail creates `/etc/apparmor.d/firejail-default` profile. Load and enforce it in apparmor by:
```shell
sudo apparmor_parser -r /etc/apparmor.d/firejail-default
sudo aa-enforce /etc/apparmor.d/firejail-default
```
Then apparmor will be used for firejail profiles that have the `apparmor` line.

## Using Firejail by Default
To use Firejail by default for all applications for which it has profiles, run the `firecfg`.
```shell
sudo firecfg
```
It creates symbolic links in `/usr/local/bin` that point to `/usr/bin/firejail`, for all programs with default firejail profiles.
In order for this to work, `/usr/local/bin` must be before `/usr/bin/` in your `PATH`.
To manually do this: `sudo ln -s /usr/bin/firejail /usr/local/bin/<program name>`
## Exceptions
Some GUI application launchers (.desktop files) are coded using absolute paths to an executable, which circumvents firejail's symlink method of ensuring that it is being used. The `firecfg` tool includes an option to over-ride this on a per-user basis by copying the .desktop files from `/usr/share/applications/*.desktop` to `~/.local/share/applications/` and replacing the absolute paths with simple file names.
```shell
firecfg --fix
```
There may cases for which you need to manually modify the EXEC line of the .desktop file in `~/.local/share/applications/` to explicitly call Firejail. To be on the safe side, apply both methods to all software.
After everything, verify if the programs are using firejail by:
```shell
firejail --list
```
## Clean-up
Remove the software that you do not want to be sandboxed from `/usr/local/bin` and `~/.local/share/applications/`.
Must-be-sandboxed softwares:
+ Web Browsers: firefox, firefox-esr, chromium
+ Office: libreoffice suite
+ Mail: evolution, thunderbird
+ Image: eog (gnome photo viewer), inkscape, gimp
+ Video: vlc, totem
+ PDF readers: evince, qpdf
+ p2p: transmission, qbittorrent
+ Gnome-specific stuff: documents, photos, music, video but not the keyring and certificate stuff

In general, if a software has internet access or gets input from internet-downloaded files, sandbox it.
