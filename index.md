## Arch Linux Installation Documentation



### Installing Arch Linux in VMware Workstation


- Open VMware Workstation and click on _File_ and then _New Virtual Machine_. Choose _Typical_ 
- Under _Install Operating System from_, click _Use ISO image_, and select arch linux that was downloaded
- Click _Linux_ under _Guest Operating System_, and then select _Other Linux 5.x and later kernel 64-bit_, click _next_
- Choose at least _20GB_ 
- Right click under _Library_ and click _Settings_, click _Options_. Under _Advanced_ change the firmware type to _UEFI_
- Start the VM
- To make font bigger for easier reading: # setfont /usr/share/kbd/consolefonts/ter-g32n.psf.gz
- To verify you are in UEFI mode: # ls /sys/firmware/efi/efivars
- To verify you are connect to Internet: # ping -c 4 www.linuxconfig.org
- Update the system clock: # timedatectl set-ntp true
- Partition the disk, to see current disk layout: # lsblk
- Create partitions: # cfdisk /dev/sda
- Select _gpt_ for label type and press enter
- Partition 1: Press enter to select New, then type 500M and press enter to create the EFI partition(sda1). Press the right arrow to select Type and change the partition type to EFI System.
- Partition 2: Press down to select Free space, then press enter on New to create the root partition(sda2), enter 18.5G for Partition size and press enter.
- Partition 3:Press down to select Free space again and press enter on New to create the swap partition(sda3). Enter 1G for Partition size and press enter. Press the right arrow and press enter to select Type then select Linux swap for the partition type.
- Use the arrow keys to select Write and press enter. Type yes and press enter to confirm that you want to write the partition table to the disk. Now select Quit and press enter to exit cfdisk.
- Create file systems for paritions
- Create _swap_ file system: # mkswap /dev/sda3 #swapon /dev/sda3
- Create _root_ file system: # mkfs.ext4 /dev/sda2
- Create _EFI_ file system: # mkfs.fat -F32 /dev/sda1
- Mount _root_ partition: # mount /dev/sda2 /mnt
- Create _boot_ directory to mount _EFI_ partition: # mkdir /mnt/boot
- Mount _EFI_ partition to that directory: # mount /dev/sda1 /mnt/boot
- Install essential packages for base system: # pacstrap /mnt base linux linux-firmware
- Generate _fstab_ file: # genfstab -U /mnt >> /mnt/etc/fstab
- Chroot into it: # arch-chroot /mnt
- Customize timezone: # ln -sf /usr/share/zoneinfo/US/Central /etc/localtime
- Install text editor nano: $ pacman -S nano
- Edit locale: # locale-gen
- Edit hostname: # nano /etc/hostname iheartcodi
- Configure networking: # systemctl enable systemd-networkd # systemctl enable systemd-resolved
- Determine network interface: # ip addr
- Edit _/etc/systemd/network/20-wired.network_ and enter [Match]
Name=ens33 [Network] DHCP=yes
- Set password for your root user: # passwd
- Install bootloader _grub_ : # pacman -S grub efibootmgr
- Then # grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
- Generate main _grub_: # grub-mkconfig -o /boot/grub/grub.cfg
- Unmount partitions and reboot system: # exit # unmount -R /mnt #reboot

### Install DE

- pacman -S xorg-server xorg-apps xorg-xnit
- Select all
- pacman -S lightdm lightdm-gtk-greeter
- systemctl enable lightdm.service
- pacman -S cutefish
- reboot

### Adding users

- Use DE to create users and set passwords in settings and set as _administrator_ privilege
- To set login where users will change password upon first login, make their password expire:# passwd -e sal # passwd -e codi
- Should receive prompt that says.. passwd: password expiry information changed.

### Install zsh shell

- pacman -S zsh
- type just _zsh_ in terminal to test it

### Install ssh and get into class gateway

- pacman -Sy openssh
- ssh -pt53997 gnt53997@129.244.245.21 and enter password

### Add color code to terminal

- Copy files in case: # cp .bashrc .bashrc.backup # cp /etc/bash.bashrc /etc/bash.bashrc.backup
- Download online files with color code in files
- cd Desktop
- mv bash.bashrc /etc/bash.bashrc
- mv .bashrc ~/.bashrc
- type _exit_ to restart terminal

### Set system to boot GUI DE

- Check if default is on GUI: # systemctl get-default
- If it's not: # systemctl set-default graphical.target and then # reboot

### Add Aliases

- To check what aliases you have: # alias
- To add: # alias {shortcut}=[command]

