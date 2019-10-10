# Manjaro KDE Install (for XPS 15 9570)
All things done while installing Manjaro KDE to have a consistant installation next time.

# Table of contents.
- [Creating a bootable USB.](#creating-a-bootable-usb)
- [Making the system read the USB.](#making-the-system-read-the-usb)
- [Select the USB on boot.](#select-the-usb-on-boot)
- [Installing Yay.](#installing-yay)
- [Installing Adapta KDE theme and icons.](#installing-adapta-kde-theme-and-icons)
- [Installing Visual Studio Code](#installing-visual-studio-code)
- [Installing DisplayLink](#installing-displaylink)
- [Fixing suspend bug.](#fixing-suspend-bug)

# Creating a bootable USB.
### Prerequiries:
- [Download](https://manjaro.org/download/) the Manjaro KDE ISO.
- [Download](https://rufus.ie/) RUFUS.

### Instructions:
1. Open RUFUS and select the USB to install the ISO on.
2. Select the ISO and start the process.
3. RUFUS will prompt to choose a image version. Select **DD-Image**

# Making the system read the USB.
### Instructions:
1. Boot into BIOS (F2 while booting the system)
2. Disable Secure Boot
3. Set drive on AHCI instead of RAID **(WARNING: this will break any existing OS).**
4. Save and Exit.

# Select the USB on boot.
### Instructions:
1. Boot into device selection (F12 while booting the system)
2. Select the USB.

**WARNING: reboot the system before continueing**

# Installing Yay.
To install AUR packages it is recommended to install *Yay* as this makes live a lot easier for installing.

### Installation:
Execute the following command:
~~~~
sudo pacman -Sy yay
~~~~

# Installing Adapta KDE theme and icons.
> Theme: https://github.com/PapirusDevelopmentTeam/adapta-kde
>
> Icons: https://github.com/PapirusDevelopmentTeam/papirus-icon-theme

### Prerequires:
- Install *kvantum-manjaro* using Octopi.
  - When extra dependencies are prompted, don't select any of them.
- Run the following command to install the *theme*
~~~~
wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/adapta-kde/master/install.sh | sh
~~~~
- Run the following command to install the *icons*
~~~~
wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/papirus-icon-theme/master/install.sh | sh
~~~~

### Installation:
Kvantum Setup:
1. Open *Kvantum* using the start menu.
2. Go to *Change/Delete theme* and select *AdaptaNokto* in the dropdown.
3. Click *Use this theme*

KDE setup:
1. Open *System Settings* using the start menu.
2. 
- Go to *Workspace Theme* and select *Adapta* & *Apply*
- Go to *Workspace Theme* > *Desktop Theme* and select *Adapta* & *Apply*
- Go to *Colors* and select *Adapta Nokto* & *Apply*
- Go to *Application Style* > *Widget Style* > Make sure *kvantum* is selected.
- Go to *Application Style* > *Window Decorations* > *Configure Adapta* & *Apply*
- Go to *Icons* and select *Papirus-Dark* & *Apply*

Firefox setup:
> Extension: https://addons.mozilla.org/nl/firefox/addon/adapta-theme-webextension/

1. Install the extension.

# Installing Visual Studio Code
> Reference: https://wiki.archlinux.org/index.php/Visual_Studio_Code

### Installation:
1. Use the command:
~~~~
yay visual-studio-code-bin
~~~~
2. Type the number of the package with name *visual-studio-code-bin*
3. Finish the installation steps.

# Installing DisplayLink
> Reference: https://wiki.archlinux.org/index.php/DisplayLink#USB_3.0_DL-6xxx,_DL-5xxx,_DL-41xx,_DL-3xxx_Devices

### Installation:
1. Update the current Linux kernel version to the latest build of that version using:
~~~~
sudo pacman -Syyu
~~~~
This will take a while and will update all other packages.

2. Type *uname -r*. This displays what Linux version you are using. The first 3 digits mather.

3. Reboot the system (not shutdown!)

4. Install the Linux headers for the corresponding kernel version using:
~~~~
pacman -S linux419-headers
~~~~
**Replace 419 by the first 3 digits of the output of uname -r**

5. Reboot the system (not shutdown!)
6. Install *evdi* from https://aur.archlinux.org/packages/evdi-git/ (use *yay evdi-git*)
7. Install *DisplayLink* from https://aur.archlinux.org/packages/displaylink/ (use *yay displaylink*)
8. Open */usr/share/X11/xorg.conf.d/20-evdidevice.conf* and add:
~~~~
Section "OutputClass"
	Identifier "DisplayLink"
	MatchDriver "evdi"
	Driver "modesetting"
	Option  "AccelMethod" "none"
EndSection
~~~~
9. Reboot the system
10. Enable the *DisplayLink* service using:
~~~~
systemctl enable displaylink.service
~~~~
11. Start the *DisplayLink* service using=
~~~~
systemctl start displaylink.service
~~~~
12. Go to System Settings > Search *compositor* > Change *Rendering Backend* to **XRender**

# Fixing suspend bug.
Manjaro has *s2idle* as default sleep operation. You can compare this to *Connected Standby* in Windows. 
This causes the system to wake up from sleep when closing the laptop lid.

### Check sleep type:
You can check what type of sleep mode is enabled using:
~~~~
cat /sys/power/mem_sleep 
~~~~

### Fix the issue:
To fix the issue you need to add *mem_sleep_default=deep* to your kernel parameters:

1. Open */etc/default/grub*
2. Find the following line:
~~~~
GRUB_CMDLINE_LINUX_DEFAULT=
~~~~
3. Add the following content: (add a space after the existing code)
~~~~
mem_sleep_default=deep
~~~~
4. Regenerate GRUB using:
~~~~
grub-mkconfig -o /boot/grub/grub.cfg
~~~~
