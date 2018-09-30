“physical block size is 2048 bytes, but Linux says it is 512” when formatting USB - How to create a bootable USB without this error




```
gnome-disks
```


Make sure the device has no mounted filesystem and unmount it if necessary, for example:

```
udisksctl unmount -b /dev/sdb1
```

Destroy existing partition table:

```
sudo sgdisk --zap-all /dev/sdb
```

Create new GPT:
```
sudo sgdisk --new=1:0:0 --typecode=1:ef00 /dev/sdb
```
Format as FAT32:
```
sudo mkfs.vfat -F32 /dev/sdb1
```
Check it:
```
sudo fdisk -l /dev/sdb
```
Should output something like:
```
Device     Start      End  Sectors  Size Type
/dev/sdb1   2048 15663070 15661023  7.5G EFI System
```
Mount the drive and extract iso onto it, replacing 'name-of-iso' with the actual filename of the iso you downloaded earlier
```
sudo mount -t vfat /dev/sdb1 /mnt
sudo 7z x name-of-iso -o/mnt/
```
Unmount
```
sudo umount /mnt
```
Now reboot & enjoy Ubuntu ^_^
