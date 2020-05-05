## Firmware
```shell
sudo apt-get install firmware-linux-free firmware-misc-nonfree intel-microcode fwupd mesa-va-drivers vainfo vulkan-tools mesa-vulkan-drivers mesa-utils
```
Depending on the Intel chipset generation, you also **need to install** either `i965-va-driver`(older) or  `intel-media-va-driver`(newer) for VAAPI support. `intel-media-va-driver` might **need to be enabled manually**, see the package docs. Installing both does not do any harm.

## Desktop Environment
### Debian Desktop Base
```shell
sudo apt-get install xorg xserver-xorg-input-all xserver-xorg-video-all desktop-base xdg-utils anacron iw sudo
```
eject libnss-mdns libu2f-udev alsa-utils

**Note:** These give you minimal base for the desktop environment installation. Packages are selected packages from `task-desktop` metapackage:

### GNOME 3
+ Base:
```shell
sudo apt-get install gnome-core xdg-user-dirs-gtk libproxy1-plugin-networkmanager network-manager-gnome seahorse evolution evolution-plugins file-roller gedit-plugins gnome-calendar gnome-contacts gnome-clocks gnome-todo gnome-tweaks nautilus-sendto totem-plugins libreoffice-calc libreoffice-impress libreoffice-writer libreoffice-gnome firefox-esr
```
**Note:** These are the selected packages from `gnome` metapackage. Essential GNOME packages are included in `gnome-core` metapackage.

+ Recommended:
```shell
sudo apt-get install dconf-editor cheese gnome-power-manager gnome-screenshot gnome-sound-recorder gnome-color-manager gnome-shell-extension-disconnect-wifi gnome-shell-extension-suspend-button gnome-shell-extension-dashtodock gstreamer1.0-vaapi gstreamer1.0-libav gstreamer1.0-plugins-good gstreamer1.0-plugins-ugly ffmpeg zip unzip unrar-free
```

+ Suggested:
```shell
sudo apt-get install mpv inkscape gimp chromium chromium-sandbox telegram-desktop
```

+ Extras: Tools **[Won't Install by Default]**:
```shell
sudo apt-get install gnome-photos transmission-gtk simple-scan
```

+ Extras: Network Discovery/UPnP **[Won't Install by Default]**:
```shell
sudo apt-get install avahi-daemon cups-pk-helper rygel-playbin rygel-tracker
```

## Laptop
```shell
sudo apt-get install tlp tlp-rdw
```
**Note:** An alternative would be `laptop-mode-tools`, but TLP is newer and has some advantages.

## Security
```shell
sudo apt-get install rng-tools5 openvpn network-manager-openvpn network-manager-openvpn-gnome chkrootkit apparmor apparmor-utils firejail firejail-profiles dnscrypt-proxy usbguard usbguard-applet-qt keepassxc logwatch
```

**Note:** If on Debian, `rng-tools`, has different versions (offical vs unofficial) in deb repos, the official version is called `rng-tools5`. On other distros, you probably just need `rng-tools`.
**Note:** To install `Tor Browser`, check the Debian website [https://wiki.debian.org/TorBrowser](https://wiki.debian.org/TorBrowser).

## Sys Admin Tools
+ Network:
```shell
sudo apt-get install openssh-client ssh-tools filezilla ldnsutils whois traceroute dnsutils nmap net-tools netcat-openbsd wireshark-gtk
```
+ Other:
```shell
sudo apt-get install efibootmgr efitools dmidecode lshw pciutils hardinfo psensor smartmontools rsync clonezilla parted e2fsprogs htop
```

## Filesystem Support
```shell
sudo apt-get install exfat-utils exfat-fuse dosfstools hfsplus hfsutils
```

## Fonts & Typesetting
```shell
sudo apt-get install fontconfig texlive-base texlive-bibtex-extra texlive-fonts-recommended texlive-science texlive-latex-base texlive-latex-recommended texlive-lang-english lmodern fonts-lmodern cm-super fonts-urw-base35 fonts-texgyre fonts-freefont-otf fonts-freefont-ttf fonts-dejavu fonts-roboto fonts-ibm-plex fonts-liberation fonts-liberation2 fonts-open-sans biber bibutils
```

**Note:** `latexila` might be called `GNOME Latex` in future releases.
**Note:** `fonts-liberation` and `fonts-liberation2` package solve some of the font rendering problems in `chromium`.

## Localization
```shell
sudo apt-get install hunspell-en-us hunspell-en-gb hunspell-tr hyphen-en-us hyphen-en-gb mythes-en-us firefox-esr-l10n-all libreoffice-l10n-en-gb chromium-l10n
```

## Package Managers/Build Tools
```shell
sudo apt-get install build-essential gcc g++ make automake libtool aptitude
```

## Programming/Dev Tools
```shell
sudo apt-get install nano git python3 python3-flake8 flake8 python3-pydocstyle pydocstyle python3-jedi nodejs yarnpkg npm
```

## Phone Access
```shell
sudo apt-get install adb fastboot jmtpfs mtp-tools libimobiledevice-utils ideviceinstaller ifuse
```

## Virtualization
```sheel
sudo apt-get install qemu-system-x86 qemu-kvm libvirt-clients libvirt-daemon-system virtinst virt-viewer virt-manager bridge-utils
```
**Note:** On Debian 10, `qemu` package is dummy and split into architecture specific packages. `qemu-system-x86` covers both i386 and x86_64.

## Other
### Cryptocurrency
Probably not in repos: `Electrum Bitcoin Wallet` and `MyCrypto Ethereum Wallet`. Get them from their official websites. If getting the appimages, place them under `/usr/local/bin/`.

### Atom
+ Get it from the official atom debian repos.
Useful plugins from APM:
```
editorconfig autocomplete-python linter linter-flake8 linter-pydocstyle language-solidity
```
+ Increase the `linter` checking interval to a higher value, e.g. `1000ms`.
+ `Editorconfig` conflicts with the `whitespace` core plugin, therefore disable the latter.
