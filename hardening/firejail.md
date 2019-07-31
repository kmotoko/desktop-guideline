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
To manually do this: `sudo ln -s /usr/bin/firejail /usr/local/bin/<program name>`.

## Exceptions
+ Atom Editor: On `Debian 10` with `firejail 0.9.58.2`, `firecfg` does not detect `atom` editor. Thus do it manually:
```shell
sudo ln -s /usr/bin/firejail /usr/local/bin/atom
cp /usr/share/applications/atom.desktop ~/.local/share/applications/
```
Then edit `EXEC` line in `~/.local/share/applications/atom.desktop` and remove the absolute path.
Also atom github integration requires a bit of tweaking. Not sure if all the following requires, try to restrict it in the future. Put the following into `/etc/firejail/atom.local`:

```
# GitHub integration requires
ignore nodbus
ignore noexec /tmp
```

+ Electrum Bitcoin Wallet (Appimage): Firejail has an electrum profile. However, since we used the appimage for the setup, it needs to be configured. Note that desktop file does not recognize `~` or `$HOME` for the path, therefore replace the `<USERNAME>` or the whole path if not using the home directory. Add the `electrum.desktop` to `~/.local/share/applications`:

```
[Desktop Entry]
Name=Electrum
Comment=Electrum Bitcoin Wallet.
GenericName=Bitcoin Wallet
Exec=firejail --appimage --profile=/etc/firejail/electrum.profile /home/<USERNAME>/Appimages/electrum.AppImage
Type=Application
StartupNotify=true

```

+ Generic: In newer firejail versions (at least on firejail 0.9.58.2), `sudo firecfg` also runs the `--fix`. Therefore this step is not necessary, but leaving for reference.
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
Remove the software that you do not want to be sandboxed from `/usr/local/bin`. You might want to exclude `keepassxc` as it is sensitive and preventing data corruption is important.
