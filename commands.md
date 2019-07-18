## ClamAV
```shell
sudo systemctl stop clamav-freshclam.service
sudo freshclam
sudo systemctl start clamav-freshclam.service
sudo clamscan -zov --detect-pua=yes --bytecode-timeout=900000 --max-filesize=1000M --max-dir-recursion=100 -r / -l $HOME/clamav_log_
```

## Chkrootkit
```shell
sudo chkrootkit -r /
```

## Rkhunter
```shell
# Below for rkhunter
sudo rkhunter --versioncheck
sudo rkhunter --update
sudo rkhunter --checkall
# After first check, examine the output
# Then do properties update if nothing seems wrong and check again
sudo rkhunter --propupd
sudo rkhunter --checkall
```

## Fsck on LUKS
```shell
# IMPORTANT: USE LIVE USB FOR FSCK
# DO NOT RUN FSCK ON MOUNTED FS
sudo modprobe dm-crypt
sudo cryptsetup luksOpen /dev/sdX3 crypt3
# LVs are usually automatically found
# So below two may not be necessary
sudo vgscan --mknodes
sudo vgchange -ay

# make sure it is ext4, check the output
sudo fsck -NV /dev/mapper/ubuntu--vg-root
# after you are sure it is ext4
sudo fsck.ext4 -nv /dev/mapper/ubuntu--vg-root
sudo fsck.ext4 -nfv /dev/mapper/ubuntu--vg-root

# also check the minimal boot partition
sudo fsck -NV /dev/sdX2
# if it is ext2
sudo fsck.ext2 -nv /dev/sdX2
sudo fsck.ext2 -nfv /dev/sdX2

# deactivate LVs
sync
sudo vgchange -an
# lock LUKS device
sudo cryptsetup luksClose crypt3
# follow spin-down instructions
```

## LUKS Header Backup
```shell
# First identify the encrypted partition
lsblk
# Then run the command below for all existing partitions until you identify it
# When identified, it should say 'LUKS header info for...'
# Otherwise it says 'not a valid LUKS device'
sudo cryptsetup -v luksDump /dev/sdX1
# After you identified the partition,
# Use cryptsetups backup option
# <device> is the partition containing the LUKS volume
# <file> is the path to backup file to be generated
# IMPORTANT: <file> should be in an encrypted and separate device for safety
sudo cryptsetup luksHeaderBackup --header-backup-file <file> <device>
sudo cryptsetup luksHeaderBackup /dev/<device> --header-backup-file <file>.img
# Ex: sudo cryptsetup luksHeaderBackup /dev/sdxy --header-backup-file /root/partition_name.img
```

## Eject/Safely Remove
```shell
#for NON-LUKS devices:
sync
sudo umount /dev/sdX1
udisksctl unmount -b /dev/sdX1
udisksctl power-off -b /dev/sdX
lsblk

#for LUKS devices:
sync
sudo umount /media/user/my_device
sudo cryptsetup luksClose my_encrypted_volume
udisksctl unmount -b /dev/sdX1
udisksctl power-off -b /dev/sdX
lsblk
```

## Folder Renamer
@TODO: You should first rename the deeper folders in the tree
```shell
# first rename directories
# can add -n option to rename for DRY-RUN
find $HOME/Documents/ -depth -type d | rename -nono -verbose "s/[&%'!()?=><:]//g"
find $HOME/Documents/ -depth -type d | rename -nono -verbose "s/[\s,;+]/_/g"
find $HOME/Documents/ -depth -type d | rename -nono -verbose "s/_-_/_/g"
find $HOME/Desktop/ -depth -type d | rename -nono -verbose "s/[&%'!()?=><:]//g"
find $HOME/Desktop/ -depth -type d | rename -nono -verbose "s/[\s,;+]/_/g"
find $HOME/Desktop/ -depth -type d | rename -nono -verbose "s/_-_/_/g"
find $HOME/Pictures/ -depth -type d | rename -nono -verbose "s/[&%'!()?=><:]//g"
find $HOME/Pictures/ -depth -type d | rename -nono -verbose "s/[\s,;+]/_/g"
find $HOME/Pictures/ -depth -type d | rename -nono -verbose "s/_-_/_/g"
```

## File Renamer
```shell
find $HOME/Documents/ -type f | rename -nono -verbose "s/[&%'!()?=><:]//g"
find $HOME/Documents/ -type f | rename -nono -verbose "s/[\s,;+]/_/g"
find $HOME/Documents/ -type f | rename -nono -verbose "s/_-_/_/g"
find $HOME/Documents/ -type f | rename -nono -verbose "s/__/_/g"
find $HOME/Desktop/ -type f | rename -nono -verbose "s/[&%'!()?=><:]//g"
find $HOME/Desktop/ -type f | rename -nono -verbose "s/[\s,;+]/_/g"
find $HOME/Desktop/ -type f | rename -nono -verbose "s/_-_/_/g"
find $HOME/Desktop/ -type f | rename -nono -verbose "s/__/_/g"
find $HOME/Pictures/ -type f | rename -nono -verbose "s/[&%'!()?=><:]//g"
find $HOME/Pictures/ -type f | rename -nono -verbose "s/[\s,;+]/_/g"
find $HOME/Pictures/ -type f | rename -nono -verbose "s/_-_/_/g"
find $HOME/Pictures/ -type f | rename -nono -verbose "s/__/_/g"
```

# Rsync
```shell
# With rsync, all exclude (or include!) paths beginning with / are are anchored to the "root of transfer", not the root directory.
# if the pattern ends with a / then it will only match a directory, not a regular file, symlink, or device.
rsync -rltv --delete --delete-excluded --exclude '/.cache/' </source/path/> </destination/path/>
# you might run it in reverse and compare checksums with dry-run
rsync -ncrltv --exclude '.cache' </destination/path/> </source/path/>
```

## Smartctl
```shell
#required package name 'smartmontools'
#To get the wearout level
sudo smartctl -a /dev/sda | grep Media_Wearout_Indicator
#Example output below:
#231 SSD_Life_Left 0x0013   100   100   000    Pre-fail  Always - 100
#You should look at the 'Value', which is the normalized value
#100 means full life, 0 is the worst.
#A value of 20 should ALERT you.

#To show complete SSD information:
sudo smartctl -a /dev/sda
```

## Other
Find root owned files:
```shell
# list files that are owned by root
# uid 0 is the root
find /home/USERID -uid 0
```
