## Directions to enable T2 Support AFTER THE FACT

1. Begin by adding this to the bottom of /etc/pacman.conf and refresh your repositories: 

'''
[arch-mact2]
Server = https://mirror.funami.tech/arch-mact2/os/x86_64
SigLevel = Never
'''

2. Run "sudo pacman -Syy linux-t2". You can install the xanmod t2 kernel with its designated package name as well, if desired.




3. Add apple-bce to the MODULES list in /etc/mkinitcpio.conf, and then run mkinitcpio -P

4a. For Grub, install a bootloader, GRUB is easier, but you can also use systemd-boot. Don't do both.

    Installing Grub:
        Edit /etc/default/grub, you'll need to install a text editor (i.e. vim or nano) with pacman -S PACKAGE_NAME for this step.
        On the line with GRUB_CMDLINE_LINUX="quiet splash", add the following kernel parameters: intel_iommu=on iommu=pt pcie_ports=compat
        Run grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB --removable.
        grub-mkconfig -o /boot/grub/grub.cfg

4b. For Systemd-boot, 

    Follow the Arch wiki's instructions. You will want --path=/boot/efi as an argument to bootctl if you mounted your EFI partition there. Also make sure you configure it to boot the linux-t2 kernel.
    Install a text editor (i.e. pacman -S vim or pacman -S nano), and make the following edit for .conf files in /boot/efi/loader/entries/.
    Add intel_iommu=on iommu=pt pcie_ports=compat to the options line to add those kernel parameters.



5. Reboot and enjoy



## Optional
# Building the Kernel
git clone https://github.com/NoaHimesaka1873/linux-t2-arch
cd linux-t2-arch
makepkg -si


