# Arch Linux GRUB Customization

## Overview
This note documents the changes made to GRUB configuration files (`/etc/grub.d/`) and related files to achieve a working custom GRUB menu for Arch Linux and Windows Boot Manager on a system with a separate `/boot` partition.

---

## 1. System Layout

| Partition       | Mount Point | FS Type | Purpose |
|-----------------|------------|--------|---------|
| nvme0n1p1       | /boot      | FAT32  | Boot partition (GRUB, kernels, initramfs, microcode) |
| nvme0n1p2       | /          | ext4   | Root filesystem |
| nvme0n1p3       | /home      | ext4   | User data |

> **Note:** `/boot` is a separate FAT32 partition (p1). All boot files reside here.

---

## 2. Changes to `/etc/grub.d/40_custom`

Custom menu entries were added for:

- Arch Linux (regular kernel)
- Arch Linux LTS
- Windows Boot Manager
- UEFI Firmware Settings

```bash
#!/bin/sh
exec tail -n +3 $0

menuentry "Arch Linux" {
    insmod part_gpt
    insmod fat
    search --no-floppy --fs-uuid --set=root 21B8-6317
    linux /vmlinuz-linux root=UUID=2a461d70-12fc-47b7-a33c-b218eeba9cbf rw quiet
    initrd /intel-ucode.img /initramfs-linux.img
}

menuentry "Arch Linux LTS" {
    insmod part_gpt
    insmod fat
    search --no-floppy --fs-uuid --set=root 21B8-6317
    linux /vmlinuz-linux-lts root=UUID=2a461d70-12fc-47b7-a33c-b218eeba9cbf rw quiet
    initrd /intel-ucode.img /initramfs-linux-lts.img
}

menuentry "Windows Boot Manager" {
    insmod part_gpt
    insmod fat
    set root='hd0,gpt1'
    search --no-floppy --fs-uuid --set=root 38C1-8AF9
    chainloader /efi/Microsoft/Boot/bootmgfw.efi
}

menuentry "UEFI Firmware Settings" {
    fwsetup
}