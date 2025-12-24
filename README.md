# Installation guide

## 1. Connecting to a wifi

If you are running the iso on a virtual machine, the virtual machine already has a provided wifi with virtual ethernet. Or if you are on a real machine, you must have a wifi device wlan0 or something.

So type this to see what is your wifi card's name.

```bash
# ip link
```

Use `iwctl` to connect to wifi

## 2. Checking if you machine is Legacy BIOS or UEFI

If you don't know what is your machine's bios version, type this.

```
# cat /sys/firmware/efi/fw_platform_size
```

If it returns as `64`, you are using a UEFI machine or if it says `32`, you are booting as UEFI 32 bits.
If it says that the file doesn't exist, that means you are booting in as Legacy BIOS.

## 3. Update your `pacman` package manager

```
# pacman -Syu
```

## 4. Installing the base system

In BusyImage, there is a file in `/usr/install/install_world`. There are two types of it, the first one is the normal `install_world` file. The normal file is for UEFI systems. The second one is `install_world_legacy_BIOS`. The file with the suffix `legacy_BIOS` is the
installer for Legacy BIOS systems. To start the installation you must define environment variables like `$UEFI_BOOT, $SWAP, $ROOT`. The UEFI_BOOT variable is the uefi boot partition if you use uefi.

Important notes:

This installer don't install the bootloader. It just a bootstrapper.

## 5. Chrooting into /mnt

```
# arch-chroot /mnt
```

### 6. Tweaking for the installation

```
vim /etc/locale.gen
```
Uncomment out your language like `en_US`.

```
locale-gen
```

```
pacman -S networkmanager grub efibootmgr
systemctl enable NetworkManager
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

This is the grub-install for Legacy BIOS

```
grub-install --target=i386-pc /dev/your-disk
grub-mkconfig -o /boot/grub/grub.cfg
```

Now we are gonna make a password for the root user.

```
passwd root
```

# Issues

## Issue 1: Grub config file issue.

If the grub.cfg file is making the select menu as Arch Linux, go to /etc/default/grub and edit the `GRUB_DISTRIBUTOR=Arch` to `GRUB_DISTRIBUTOR=BusyImage`.

# Contact us!
If you are planning to make a custom arch based linux distro, contact me on Discord user: sam2210216 or you can contact me on gmail: mmbk88221@gmail.com.
