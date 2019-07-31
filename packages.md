## Firmware/Hardware
```shell
sudo apt-get install firmware-linux-free firmware-misc-nonfree laptop-mode-tools lshw pciutils hardinfo psensor
```

Microcode for Intel processors:
```shell
sudo apt-get install intel-microcode
```

VAAPI driver for i965 chipsets:
```shell
sudo apt-get install mesa-va-drivers i965-va-driver intel-media-va-driver vainfo
```

Vulkan support:
```shell
sudo apt-get install vulkan-tools mesa-vulkan-drivers
```
## Security
```shell
sudo apt-get install openvpn network-manager-openvpn openconnect clamav chkrootkit apparmor apparmor-profiles-extra apparmor-utils firejail firetools ldnsutils dnscrypt-proxy nautilus-wipe wipe usbguard usbguard-applet-qt keepassxc logwatch
```
If on GNOME, also:
```shell
sudo apt-get install network-manager-openvpn-gnome network-manager-openconnect-gnome
```

Get `Tor Browser` from the official website.

## Filesystem/Backup
```shell
sudo apt-get install exfat-utils exfat-fuse dosfstools parted rsync smartmontools hfsplus hfsutils e2fsprogs
```

## Typesetting
```shell
sudo apt-get install texlive-base texlive-bibtex-extra texlive-fonts-recommended texlive-fonts-extra lmodern fonts-lmodern texlive-science texlive-latex-base texlive-latex-extra texlive-latex-recommended texlive-lang-english texlive-lang-french texlive-lang-german texlive-lang-greek cm-super texstudio biber bibutils
```

## Media/Misc/Daily
```shell
sudo apt-get install vlc gstreamer1.0-vaapi gstreamer1.0-libav gstreamer1.0-plugins-ugly gstreamer1.0-plugins-good ffmpeg unrar-free zip unzip qpdfview impressive pdf-presenter-console inkscape gimp chromium chromium-sandbox dconf-editor
```

## GNOME Shell
```shell
sudo apt-get install gnome-shell-extension-disconnect-wifi gnome-shell-extension-suspend-button numix-gtk-theme
```

## Localization
```shell
sudo apt-get install hunspell-en-gb hyphen-en-gb firefox-esr-l10n-en-gb libreoffice-l10n-en-gb chromium-l10n
```

## Package Managers/Build Tools
```shell
sudo apt-get install build-essential gcc g++ make automake libtool aptitude
```

## Programming/Dev Tools
```shell
sudo apt-get install python3 python3-pep8 python3-pep8-naming python3-flake8 git filezilla openssh-client smbclient cifs-utils whois traceroute dnsutils
```

## Phone Access
```shell
sudo apt-get install adb fastboot jmtpfs mtp-tools libimobiledevice-utils ideviceinstaller ifuse
```

## Cryptocurrency
Probably not in repos: `Electrum Bitcoin Wallet` and `MyCrypto Ethereum Wallet`. Get them from their official websites.

## Atom
Get it from the official atom debian repos.

## Virtualbox
Get it from the official Virtualbox website.

## Wire
Get it from the official Wire website.
