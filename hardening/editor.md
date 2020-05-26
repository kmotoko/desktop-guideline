## Text Editor
### Use Restricted Editor for Sudoers
Use `rnano` (restricted nano) instead of regular `nano` for `sudoer` operations. Add/edit the following to `/etc/sudoers`:
```
Defaults editor=/usr/bin/rnano
```
### Change System-Wide Default Editor
Change it to `nano` via `sudo update-alternatives --config editor`.
