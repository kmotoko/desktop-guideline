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
Also atom git/github integration requires a bit of tweaking. Also, the trash folder is masked by default, unmask it. Put the following into `/etc/firejail/atom.local`:

```
# Bring the trash func back
noblacklist ${HOME}/.local/share/Trash

# Git/GitHub integration requires
noblacklist ${HOME}/.config/git
noblacklist ${HOME}/.gitconfig
noblacklist ${HOME}/.git-credentials
ignore nodbus
ignore noexec /tmp

# Other fixes
noblacklist ${HOME}/.pythonrc.py

```

+ Electrum Bitcoin Wallet (Appimage): Firejail has an electrum profile. However, since we used the appimage for the setup, it needs to be configured. Note that desktop file does not recognize `~` or `$HOME` for the path, therefore replace the `<USERNAME>` or the whole path if not using the home directory. Add the `electrum.desktop` to `~/.local/share/applications`:

```
[Desktop Entry]
Name=Electrum
Comment=Electrum Bitcoin Wallet.
GenericName=Bitcoin Wallet
Exec=firejail --appimage --profile=/etc/firejail/electrum.profile /usr/local/bin/electrum.AppImage
Type=Application
StartupNotify=true

```

Note that the path is the absolute path, but used with firejail command. Also, it is important to keep Appimages outside the HOME dir, to prevent unintended/malicious changes. The best location for executables installed outside the package manager is `/usr/local/bin/` for all users, and `/usr/local/sbin/` for admin-only use.

+ Inkscape: It needs the Python interpreters to work properly. they are disabled at least in 0.9.58. Add the following to `/etc/firejail/inkscape.local`:

```
# Enable Python interpreters for Inkscape

# Python 2
noblacklist ${PATH}/python2*
noblacklist /usr/include/python2*
noblacklist /usr/lib/python2*
noblacklist /usr/local/lib/python2*
noblacklist /usr/share/python2*

# Python 3
noblacklist ${PATH}/python3*
noblacklist /usr/include/python3*
noblacklist /usr/lib/python3*
noblacklist /usr/local/lib/python3*
noblacklist /usr/share/python3*

# Exporting GIMP .xcf requires:
noblacklist ${HOME}/.config/GIMP
noblacklist ${HOME}/.gimp*
read-only ${HOME}/.config/GIMP
read-only ${HOME}/.gimp*

```

+ Deny Access to Sensitive Data: By adding the following to `/etc/firejail/globals.local`, all sandboxed applications can be prevented from accessing the directory with sensitive data (replace the path):

```
blacklist ${HOME}/path/to/sensitive
nowhitelist ${HOME}/path/to/sensitive

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
Remove the software that you do not want to be sandboxed from `/usr/local/bin`. You might want to exclude `keepassxc` as it is sensitive and preventing the data corruption is important.

## DPKG Hook
Add a DPKG hook so that newly installed software are automatically sandboxed.

+ Add the following to `/etc/apt/apt.conf.d/85firecfg`:
```
# Run firecfg after apt-get install/upgrade/remove
DPkg::Post-Invoke {"/usr/local/sbin/firejail_autorun.sh";};
```

Note: Pay attention if there is a hook which runs a HIDS (e.g. rkhunter DPKG hook, which is `90rkhunter`). It is better to run firecfg before HIDS is run.

+ Add the following to `/usr/local/sbin/firejail_autorun.sh`:
```shell
#!/bin/sh
# Used after apt-get upgrade/install/remove
# In order to update the sandboxed software list

# Exit script as soon as a command fails.
set -o errexit

# Executes cleanup function at script exit.
trap cleanup EXIT

# Variables
TIMESTAMP=$(date '+%Y%m%d%H%M%S')
OUTPUT="/tmp/firejail_autorun_$TIMESTAMP"

# Cleanup fnc
cleanup() {
    if [ -w $OUTPUT ]
    then
        rm -v $OUTPUT
    fi
}

# Sandbox newly installed apps
if [ -x /usr/bin/firecfg ]
then
    firecfg > $OUTPUT
fi

# Blacklist the following software
if [ -L /usr/local/bin/keepassxc ]
then
    echo >> $OUTPUT
    rm -v /usr/local/bin/keepassxc >> $OUTPUT
fi

if [ -L /usr/local/bin/keepassx ]
then
    echo >> $OUTPUT
    rm -v /usr/local/bin/keepassx >> $OUTPUT
fi

if [ -L /usr/local/bin/keepass ]
then
    echo >> $OUTPUT
    rm -v /usr/local/bin/keepass >> $OUTPUT
fi

# Atom Editor is disabled in firecfg v0.9.58.2 @ Debian 10
# Manually make a symlink
if [ ! -L /usr/local/bin/atom ]
then
    echo >> $OUTPUT
    ln -sv /usr/bin/firejail /usr/local/bin/atom >> $OUTPUT
fi

if [ -r $OUTPUT ]
then
    cat $OUTPUT | mail -s "Firejail DPKG Post-Invoke" root@localhost
fi

```

+ Make it executable `sudo chmod 754 /usr/local/sbin/firejail_autorun.sh`
