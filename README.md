# manjaro_architect_uefi-raid

sudo -s    

shred -v -n0 -z    
#ls /sys/firmware/efi    

pacman -Syy    
pacman -S openssh    
systemctl start sshd    
ssh manjaro@10.0.0.171    
sudo -s    

pacman -S gdisk    
EF00 = efi    
FD00 = RAID    

mdadm --create --verbose --level=0 --metadata=1.2 --raid-devices=2 /dev/md/md0 /dev/sda2 /dev/sdb1    
mkfs -t ext4 /dev/md/md0    
mkfs -t vfat /dev/sda1   

mkdir /boot/efi    

mount /dev/sda1 /boot/efi    
mount /dev/md/md0 /mnt   

setup    
#install   
chroot    

/etc/mkinitcpio.conf    
BINARIES=(mdmon)    
HOOKS=(base udev autodetect keyboard modconf block mdadm_udev filesystems fsck)    

#/lib/modules    
mkinitcpio -c /etc/mkinitcpio.conf -g /boot/initramfs-5.1-x86_64.img -k 5.1.1-2-MANJARO    
exit    

mdadm --detail --scan >> /mnt/etc/mdadm.conf    
grub-install --target=x86_64-efi --efi-directory=/boot/efi --root-directory=/mnt    
setup    
