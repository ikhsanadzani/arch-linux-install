# jika sudah masuk kedalam install arch linux nya langsung ikutin langkah dibawah


# CONNECT WIFI
```
iwctl
device list (buat cek wifi setiap laptop)

```

# CHECKING PARTISI
`lsblk -o name,vstype` jika ingine melihat tipe partisi nya
`lsblk` untuk melihat partisi nya

## MEMBAGI PARTISI
```
cfdisk /dev/[partisi] (untuk membentuk layout yg mah di install)
```
### MINIMAL PARTISI 
#### **MENYESUAIKAN DENGAN PENYIMPANAN YANG ADA**
```
boot = 4G [EFI system]
root = 20G [linux filesystem]
home = 100G [linux filesystem/linux server data]

```

#### kalo salah satu partisi PENTING ke hapus, langsung QUIT aja jangan di WRITE

```
lsblk (lagi)
```

# FORMATING

## BOOT
```
mkfs.vfat -F32 -S 4096 -n BOOT /dev/[partisi boot]
```

## ROOT
```
mkfs.ext4 -b 4096 /dev/[partisi root]
```


## HOME
```
mkfs.ext4 -b 4096 /dev/[partisi home]
```


# MOUNTING

## MOUNTING ROOT
```
mount /dev/[pertisi root] /mnt
```

## MOUNTING BOOT

```
mkdir -p /mnt/boot
```

```
mount -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/[partisi boot] /mnt/boot
```

## MOUNTING HOME
```
mkdir -p /mnt/home
```
```
mount /dev/[partisi home] /mnt/home
```

### INSTALL LINUX CORE
## INTEL
```
pacstrap /mnt intel-ucode base base-devel linux linux-firmware iwd git neovim 
```

## AMD
```
pacstrap /mnt amd-ucode base base-devel linux linux-firmware iwd git neovim
```

### menyimpan file yg udah di format dan di mounting

```
genfstab -U /mnt > /mnt/etc/fstab
```

### membuat folder config iwd
```
mkdir -p /mnt/var/lib/iwd
```

### terus copy file nya config wifi
```
cp /var/lib/iwd/*.psk /mnt/var/lib/iwd/
```

### terus copy file config network
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```

## lalu masuk ke mode root
```
arch-chroot /mnt
```

## lalu install desktop environment
### gnome
```
pacman -S gnome gdm networkmanager pipewire pipewire-pulse pipewire-jack
```
### plasma
```
pacman -S plasma sddm networkmanager pipewire pipewire-pulse pipewire-jack kitty
```

langsung enter aja semua

### membuat nama komputer 

jika 1 kata tidak perlu pake `""` kalo lebih menggunakan petik 2
```
echo [nama komputer] > /etc/hostname
```

### mengatur waktu 
```
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```

### generate waktu 
```
hwclock --systohc
```

### config bahasa

```
nvim /etc/locale.gen
```

lalu pencarian di nvim menggunakan `/`

```
lalu uncommenting kedua en_US
```

#### generate bahasa yg di uncommenting 
```
locale-gen
```

```
locale > /etc/locale.conf
```

### config locale
```
nvim /etc/locale.cond
```



### config file locale 
```
isi lang=C menjadi lang=en_US.UTF-8
dan isi ALL=en_US.UTF-8
```

### membuat user dan pw nya

```
useradd -m [user]
passwd [user]
```

### menambah hak sudo

```
echo 'nama_user ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/none
```

### menambahkan user kedalam grup root
```
usermod -aG wheel [user]
```

### membuat cmdline
```
mkdir /etc/cmdline.d
```

### kernel parameter
```
touch /etc/cmdline.d/{01-boot.conf,02-mods.conf,03-secs.conf,04-perf.conf,05-nets.conf,06-misc.conf}
```

### lalu config 01-boot

```
nvim /etc/cmdline.d/01-misc
```

```
root=/dev/[partisi root]
```


### lalu config 06-misc
```
nvim /etc/cmdline.d/06-boot
```

```
rw
```

### membuat folder kernel dan efi
```
mkdir kernel efi
```

### membuart file linux di efi
```
mkdir efi/linux
```

### memindahkan intel ucode dan vmlinuz ke kernel

```
mv /boot/intel/amd-ucode kernel/
mv /boot/vmlinuz-linux kernel/
```

```
ls kernel/
rm -rf intitrams-*
```

### memindahkan file mkinitcpio
```
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```

## config mkinit
```
nvim /etc/mkinitcpio.d/linux.preset
```
![WhatsApp Image 2026-02-14 at 02 31 35](https://github.com/user-attachments/assets/6a029af1-849c-4e76-8b57-edd63d9e5176)


### lalu install bootmanager
```
bootctl --path=/boot install
```

```
touch /etc/vconsole.conf
```


### generate UKI

```
mkinitcpio -P
```

## enable service
```
systemctl enable systemd-networkd.socket
```

```
systemctl enable systemd-resolved
```

```
systemctl enable sddm
```

```
systemctl enable iwd
```

```
systemctl enable NetworkManager
```

### unmount /mnt

```
umount -R /mnt
```

