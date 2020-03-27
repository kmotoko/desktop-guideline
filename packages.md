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
sudo apt-get install openvpn network-manager-openvpn openconnect chkrootkit apparmor apparmor-profiles-extra apparmor-utils firejail firetools ldnsutils dnscrypt-proxy nautilus-wipe wipe usbguard usbguard-applet-qt keepassxc logwatch
```

If on Debian, `rng-tools`, has different versions (offical vs unofficial) in deb repos, the official version is called `rng-tools5`, install it:
```shell
sudo apt-get install rng-tools5
```

On other distros, you probably just need:
```shell
sudo apt-get install rng-tools
```

If on GNOME, also:
```shell
sudo apt-get install network-manager-openvpn-gnome network-manager-openconnect-gnome
```

To install `Tor Browser`, check the Debian website [https://wiki.debian.org/TorBrowser](https://wiki.debian.org/TorBrowser).

## Filesystem/Backup
```shell
sudo apt-get install exfat-utils exfat-fuse dosfstools parted rsync smartmontools hfsplus hfsutils e2fsprogs
```

## Typesetting
```shell
sudo apt-get install fonts-liberation fonts-liberation2 texlive-base texlive-bibtex-extra texlive-fonts-recommended texlive-fonts-extra lmodern fonts-lmodern texlive-science texlive-latex-base texlive-latex-extra texlive-latex-recommended texlive-lang-english texlive-lang-french texlive-lang-german texlive-lang-greek cm-super texstudio biber bibutils
```

Note that `fonts-liberation` and `fonts-liberation2` package solve some of the font rendering problems in `chromium`.

## Media/Misc/Daily
```shell
sudo apt-get install vlc gstreamer1.0-vaapi gstreamer1.0-libav gstreamer1.0-plugins-ugly gstreamer1.0-plugins-good ffmpeg unrar-free zip unzip qpdfview impressive pdf-presenter-console inkscape gimp chromium chromium-sandbox dconf-editor telegram-desktop
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
sudo apt-get install python3 python3-flake8 flake8 python3-pydocstyle pydocstyle python3-jedi git filezilla openssh-client smbclient cifs-utils whois traceroute dnsutils
```
JS:
```shell
sudo apt-get install nodejs yarnpkg npm
```

## Phone Access
```shell
sudo apt-get install adb fastboot jmtpfs mtp-tools libimobiledevice-utils ideviceinstaller ifuse
```

## Cryptocurrency
Probably not in repos: `Electrum Bitcoin Wallet` and `MyCrypto Ethereum Wallet`. Get them from their official websites. If getting the appimages, place them under `/usr/local/bin/`.

## Atom
Get it from the official atom debian repos.
Useful plugins from APM:
```
editorconfig autocomplete-python linter linter-flake8 linter-pydocstyle language-solidity
```

+ Increase the `linter` checking interval to a higher value, e.g. `1000ms`.
+ `Editorconfig` conflicts with the `whitespace` core plugin, therefore disable the latter.

## Virtualbox
Get it from the official Virtualbox website.

## Wire
Get it from the official Wire website.
