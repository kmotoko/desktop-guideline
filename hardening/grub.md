## GRUB
### Before starting
Using `GRUB`, an attacker can change the boot parameters and can access to the command line. In order to restrict `GRUB` with a password, the following conditions should be met, otherwise it beats the purpose:
+ BIOS boot order should start with the OS, not an external drive etc.
+ BIOS boot order should be locked.
+ BIOS (inc. the boot order) should be secured with a password.

It is also important to note that the key derivation function used here is `pbkdf2`. This might be obsolete in the future, so periodically check it.
Next section is tested on `Debian 10` with `GRUB2`.

### Setting a password policy to GRUB
+ Create an hashed key:
```shell
grub-mkpasswd-pbkdf2
```
This will return an hashed key, starting with the word `grub`. Note it (inc. the`grub` part).

+ Append the following to `/etc/grub.d/40_custom`:
```
set superusers="username"
password_pbkdf2 username password
```
where `username` is your choice, and password is from the first step.

+ Update `grub`:
```shell
sudo update-grub2
```

+ Reboot and confirm that:
  + `GRUB` console requires password
  + Selecting a boot menu entry requires password
  + Changing the boot parameters requires password
