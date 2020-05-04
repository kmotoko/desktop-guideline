## Dconf
+ Although there is a way to set up system-wide defaults for Gnome and Gnome apps via `dconf` profiles, at least on `Debian 10` with `Gnome 3.30.2` it is causing problems that I could not debug (e.g. some Gnome settings were locked in the GUI, some settings were not even reflected etc.). For reference purposes, here is what you would do briefly: Check the default profile in `/usr/share/dconf/profile`, form a profile in `/etc/dconf/profile/` and lastly form a db in `/etc/dconf/db/` and do `sudo dconf update`.

+ Instead of messing with `dconf` profiles, use the following `gsettings` script to quickly set up preferences. It is **important** to note that while getting the key names from `dconf-editor`, there are two types of notations: path (with '/' slashes) and schema (with '.' dots). The one that you need for `gsettings` is the schema notation. For some keys there is also uppercase/lowercase difference between path and schema-style keys.

```shell
# Tracker and Indexer
gsettings set org.freedesktop.Tracker.Extract max-media-art-width 1024
gsettings set org.freedesktop.Tracker.Extract max-bytes 20480
gsettings set org.freedesktop.Tracker.FTS max-words-to-index 10000
gsettings set org.freedesktop.Tracker.Miner.Files low-disk-space-limit 6
gsettings set org.freedesktop.Tracker.Miner.Files throttle 5
gsettings set org.freedesktop.Tracker.Miner.Files ignored-directories "['po', 'CVS', 'core-dumps', 'lost+found', 'node_modules', 'python3-venv', 'python-venv', 'venv', 'gems', 'private', 'Private', 'secret', 'Secret', 'Backups', 'backups', 'Backup', 'backup']"

# Search
gsettings set org.gnome.desktop.search-providers disabled "['org.gnome.Software.desktop', 'seahorse.desktop', 'org.gnome.clocks.desktop', 'org.gnome.Characters.desktop', 'org.gnome.Calculator.desktop', 'org.gnome.Terminal.desktop']"

# Privacy & Security
gsettings set org.gnome.system.location enabled false
gsettings set org.gnome.desktop.privacy remove-old-temp-files true
gsettings set org.gnome.desktop.privacy old-files-age 3
gsettings set org.gnome.desktop.privacy remove-old-trash-files false
gsettings set org.gnome.desktop.privacy send-software-usage-stats false
gsettings set org.gnome.desktop.privacy report-technical-problems false
gsettings set org.gnome.desktop.privacy recent-files-max-age 30
gsettings set org.gnome.desktop.privacy hide-identity true
gsettings set org.gnome.rhythmbox.sharing enable-sharing false
gsettings set org.gnome.Maps checkin-foursquare-broadcast-twitter false
gsettings set org.gnome.Maps checkin-foursquare-privacy 'private'
gsettings set org.gnome.Maps checkin-facebook-privacy 'SELF'
gsettings set org.gnome.Maps checkin-foursquare-broadcast-facebook false
gsettings set org.gnome.desktop.media-handling autorun-never true
gsettings set org.gnome.desktop.session idle-delay 900
gsettings set org.gnome.desktop.screensaver lock-enabled true
gsettings set org.gnome.desktop.screensaver lock-delay 0
gsettings set org.gnome.desktop.screensaver idle-activation-enabled true

# Power
gsettings set org.gnome.settings-daemon.plugins.power power-button-action 'suspend'
gsettings set org.gnome.settings-daemon.plugins.power idle-dim false
gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-ac-type 'suspend'
gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-battery-timeout 1800
gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-battery-type 'suspend'
gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-ac-timeout 3600

# Housekeeping and Performance
gsettings set org.gnome.desktop.thumbnail-cache maximum-age 90
gsettings set org.gnome.desktop.thumbnail-cache maximum-size 128


# Peripherals
gsettings set org.gnome.desktop.peripherals.mouse natural-scroll true
gsettings set org.gnome.desktop.peripherals.mouse speed 0.42
gsettings set org.gnome.desktop.peripherals.touchpad natural-scroll true
gsettings set org.gnome.desktop.peripherals.touchpad tap-to-click true
gsettings set org.gnome.desktop.peripherals.touchpad two-finger-scrolling-enabled true
gsettings set org.gnome.desktop.peripherals.touchpad speed 0.67
gsettings set org.gnome.desktop.peripherals.touchpad disable-while-typing true


# Display
gsettings set org.gnome.settings-daemon.plugins.xsettings antialiasing 'rgba'
gsettings set org.gnome.settings-daemon.plugins.xsettings hinting 'slight'
gsettings set org.gnome.settings-daemon.plugins.color night-light-schedule-automatic false
gsettings set org.gnome.settings-daemon.plugins.color night-light-enabled true
gsettings set org.gnome.settings-daemon.plugins.color night-light-schedule-from 19.0
gsettings set org.gnome.settings-daemon.plugins.color night-light-schedule-to 6.0
gsettings set org.gnome.settings-daemon.plugins.color night-light-temperature 4500


# Interface
gsettings set org.gnome.desktop.interface clock-format '24h'
gsettings set org.gnome.desktop.interface clock-show-date true
gsettings set org.gnome.desktop.interface show-battery-percentage true
gsettings set org.gnome.desktop.interface text-scaling-factor 0.95
gsettings set org.gtk.Settings.FileChooser window-size "(780, 480)"
gsettings set org.gnome.nautilus.preferences always-use-location-entry true
gsettings set org.gnome.nautilus.preferences thumbnail-limit 2
gsettings set org.gnome.nautilus.icon-view default-zoom-level 'standard'


# Misc
gsettings set org.gnome.gedit.preferences.editor syntax-highlighting true
gsettings set org.gnome.gedit.preferences.editor insert-spaces true
gsettings set org.gnome.gedit.preferences.editor tabs-size 4
gsettings set org.gnome.gedit.preferences.editor bracket-matching true
gsettings set org.gnome.gedit.preferences.editor display-line-numbers true
gsettings set org.gnome.gedit.preferences.editor ensure-trailing-newline true
gsettings set org.gnome.gedit.preferences.editor wrap-mode 'none'

gsettings set org.gnome.desktop.datetime automatic-timezone false

gsettings set org.gnome.gnome-system-monitor show-whose-processes 'all'

gsettings set org.gnome.calculator button-mode 'advanced'

# GNOME Theme
gsettings set org.gnome.desktop.interface gtk-theme 'Arc'

```
