```bash
df -h /
sudo du -hxd1 / | sort -hr | head -20
sudo du -hxd1 /var | sort -hr | head -20

du -sh /var/cache/pacman/pkg
sudo pacman -Sc

sudo du -hxd1 /var | sort -hr | head -20
du -sh /var/cache/pacman/pkg
sudo pacman -Sc
sudo rm -rf /var/tmp/*

sudo journalctl --disk-usage
sudo journalctl --vacuum-size=500M  # keep only recent 500 MB
sudo rm -rf /tmp/*
rm -rf ~/.cache/*

#Clean old packages safely:
sudo pacman -Rns $(pacman -Qdtq)
```