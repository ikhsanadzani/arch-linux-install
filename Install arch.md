```
iwctl
device list (buat cek wifi setiap laptop)

```

`lsblk` untuk melihat partisi nya
```
cfdisk /dev/[partisi] (untuk membentuk layout yg mah di install)
```

```
boot = 4G [EFI system]
root = 20G [linux filesystem]
home = 100G [linux filesystem/linux server data]

```

#### kalo salah satu partisi ke hapus, langsung quit aja jangan di write itu pasti balik lagi

```
lsblk (lagi)
```

## format boot
```
mkfs.vfat -F32 -S 4096 -n BOOT /dev/[partisi boot]
```

## format root
```
mkfs.ext4 -b 4096 /dev/[partisi root]
```


## format home

```
mkfs.ext4 -b 4096 /dev/[partisi home]
```


### mounting root
```
mount /dev/[pertisi root] /mnt
```

### lalu membuat folder boot

```
mkdir -p /mnt/boot
```

### mount partisi boot
```
mount -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/[partisi boot] /mnt/boot
```

### membuat folder home 
```
mkdir -p /mnt/home
```

### mounting folder home
```
mount /dev/[partisi home] /mnt/home
```

### install packages 

## intel
```
pacstrap /mnt intel-ucode base base-devel linux linux-firmware iwd git neovim 
```

## amd
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
echo "user ALL(ALL:ALL) ALL" /etc/sudoers.d/00-user
```

### menambahkan user kedalam grup root
```
usermode -aG wheel [user]
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
nvim /etc/cmdline.d/01-boot
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
mv [cari kedua file itu]
```

```
ls kernel/
rm -rf intitrams-linux.img
```

### memindahkan file mkinitcpio
```
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```

## config mkinit
```
nvim /etc/mkinitcpio.d/linux.preset
```

![[20260212_202251.jpg]]

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

