## LUKS Header
`cryptsetup`'s `luksHeaderBackup` action stores a binary backup of the LUKS header and keyslot area. The exported file should be stored **outside the device** and in an **encrypted** manner.
```shell
sudo cryptsetup luksHeaderBackup /dev/<device> --header-backup-file <file>.img
```
