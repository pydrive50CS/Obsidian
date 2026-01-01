``` bash
sudo mount /dev/nvme0n1p2 /mnt        # root
sudo mount /dev/nvme0n1p3 /mnt/home   # optional
sudo mount /dev/nvme0n1p1 /mnt/boot   # EFI partition

sudo arch-chroot /mnt

# Install GRUB (UEFI)
sudo grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB

# Generate GRUB config with OS detection (should detect Windows automatically)
sudo grub-mkconfig -o /boot/grub/grub.cfg

efibootmgr -v

exit
sudo umount -R /mnt
reboot

```