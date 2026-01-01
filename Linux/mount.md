### Backup Drive Mount Info
UUID: 1234-ABCD-5678-EFGH
Mount Path: /mnt/Backup
Filesystem: ext4

```
### Commands used:
lsblk -f
sudo blkid -s UUID -o value /dev/sdb1
sudo mkdir -p /mnt/Backup
sudo mount UUID=$(sudo blkid -s UUID -o value /dev/sdb1) /mnt/Backup
sudo umount /mnt/Backup
sudo nano /etc/fstab
sudo e2label /dev/sdb1 Backup
sudo e2label /dev/sdb1 Backup
sudo e2label /dev/sdb1 Backup
sudo e2label /dev/sdb1 Backup
sudo mount -a
sudo mount -a
```