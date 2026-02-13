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

```
linux-zen linux-zen-headers linux-firmware iwd git neovim intel-ucode base base-devel mkinitcpio
```
