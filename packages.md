## Firmware/Hardware
```shell
sudo apt-get install firmware-linux firmware-misc-nonfree intel-microcode laptop-mode-tools lshw pciutils hardinfo psensor
```

## Security
```shell
sudo apt-get install apt-transport-https openvpn network-manager-openvpn openconnect clamav chkrootkit apparmor apparmor-profiles apparmor-profiles-extra apparmor-utils firejail firetools ldnsutils dnscrypt-proxy dnscrypt-proxy-plugins nautilus-wipe wipe
```
If on GNOME, also:
```shell
sudo apt-get install network-manager-openvpn-gnome network-manager-openconnect-gnome
```
In Debian 9, the following is in backports:
```shell
sudo apt-get install keepassxc
```
Get `Tor Browser` from the official website.

## Filesystem/Backup
```shell
sudo apt-get install exfat-utils exfat-fuse dosfstools parted rsync smartmontools tune2fs hfsplus hfsutils
```

## Typesetting
```shell
sudo apt-get install texlive-base texlive-bibtex-extra texlive-fonts-recommended texlive-fonts-extra lmodern fonts-lmodern texlive-science texlive-latex-base texlive-latex-extra texlive-latex-recommended texlive-lang-english texlive-lang-french texlive-lang-german texlive-lang-greek cm-super texstudio biber bibutils jabref
```

## Media/Misc/Daily
```shell
sudo apt-get install vlc gstreamer1.0-vaapi gstreamer1.0-libav gstreamer1.0-plugins-ugly gstreamer1.0-plugins-good ffmpeg unrar-free zip unzip qpdfview impressive pdf-presenter-console inkscape gimp chromium dconf-editor
```

## GNOME Shell
```shell
sudo apt-get install gnome-shell-extension-disconnect-wifi gnome-shell-extension-shortcuts gnome-shell-extension-suspend-button gnome-shell-extension-weather albatross-gtk-theme arc-theme blackbird-gtk-theme bluebird-gtk-theme greybird-gtk-theme gtk2-engines-murrine murrine-themes numix-gtk-theme
```

## Build Tools
```shell
sudo apt-get install build-essential gcc g++ make automake libtool checkinstall
```

## Programming/Dev Tools
```shell
sudo apt-get install python3 python3-pep8 python3-pep8-naming python3-flake8 git filezilla openssh-client smbclient cifs-utils whois traceroute dnsutils
```

## Phone Access
```shell
sudo apt-get install adb fastboot jmtpfs mtp-tools libmtp libimobiledevice libimobiledevice-utils ideviceinstaller ifuse
```

## Cryptocurrency
Probably not in repos: `Electrum Bitcoin Wallet` and `MyCrypto Ethereum Wallet`. Get them from their official websites.
