# Partitioning

## install linux minimal mempunyai 2 partisi, `BOOT` dan `ROOT`

## membuat partisi boot dan boot jika belum ada
```
cfdisk /dev/{partisi}
```

### saat membuat partisi pastikan type nya sesuai
```
BOOT = EFI system

ROOT = linux filesystem
```

# Formating
## format boot

```
mkfs.vfat -F32 -S 4096 -n BOOT /dev/{partisi boot}
```

## format root 

```
mkfs.ext4 -b 4096 /dev/{partisi root}
```

# mounting

## mounting root
```
mount /dev/{partisi root} /mnt
```

## mounting boot
```
mkdir -p /mnt/boot
```
```
mount -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/{partisi boot} /mnt/boot
```


# Package installation

## amd processor
```
pacstrap -S linux-zen linux-zen-headers linux-firmware iwd git neovim amd-ucode base base-devel mkinitcpio
```
## amd processor

```
pacstrap -S linux-zen linux-zen-headers linux-firmware iwd git neovim intel-ucode base base-devel mkinitcpio
```

# copy cofiguration
## systemd network
```
cp /etc/systemd/network/* /mnt/etc/systemd/network/
```
## iwd
```
mkdir -p /mnt/var/lib/iwd
```
```
cp -f /var/lib/iwd/*.psk /mnt/var/lib/iwd
```
## fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```

## chroot
```
arch-chroot /mnt
```

# hostname
```
```

# localtime
```
```

# locale
```
```

# adduser
```
```

# prepare boot
```
```

# kernel parameter
```
cdmline
```

# initramfs
```
mkinit
```

# bootctl
```
```

# service
```
```





