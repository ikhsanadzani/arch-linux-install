 cd /boot/
 
ls
efi  kernel  loader  vmlinuz-linux70-tkg-eevdf

mv vmlinuz-linux70-tkg-eevdf kernel/

ls
efi  kernel  loader

cd /etc/mkinitcpio.d/

ls
default.conf  linux.preset  linux70-tkg-eevdf.preset

nvim linux70-tkg-eevdf.preset 

ls
default.conf  linux.preset  linux70-tkg-eevdf.preset

mkinitcpio -P
