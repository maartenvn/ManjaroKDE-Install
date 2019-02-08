# Manjaro KDE Install (for XPS 15 9570)
All things done while installing Manjaro KDE to have a consistant installation next time.

# Creating a bootable USB.
### Prerequiries:
- Download the Manjaro KDE ISO.
- Download RUFUS.

### Instructions:
1. Open RUFUS and select the USB to install the ISO on.
2. Select the ISO and start the process.
3. RUFUS will prompt to choose a image version. Select **DD-Image**

# Making the system read the USB.
### Instructions.
1. Boot into BIOS (F2 while booting the system)
2. Disable Secure Boot
3. Set drive on AHCI instead of RAID.
4. Save and Exit.

# Select the USB on boot.
### Instructions
1. Boot into device selection (F12 while booting the system)
2. Select the USB.

# IMPORTANT: Disabling NVIDIA GPU.
Make sure to do this **the second you boot the system** (otherwise the UI will freeze)
> Reference: https://wiki.archlinux.org/index.php/Dell_XPS_15_9570

1. Open */etc/modprobe.d/blacklist.conf* (it is possible that the file doesn't exist)
2. Add these lines to the file: (this will blacklist the drivers to boot the NVIDIA GPU)

>blacklist nouveau
>
>blacklist rivafb
>
>blacklist nvidiafb
>
>blacklist rivatv
>
>blacklist nv

3. Open */etc/tmpfiles.d/nvidia_pm.conf* (it is possible that the file doesn't exist)

4. Add these lines to the file: (this will disable the NVIDIA GPU completely to save power)
> w /sys/bus/pci/devices/0000:01:00.0/power/control - - - - auto

**WARNING: reboot the system before continueing**

# Installing Adapta KDE theme and icons.
> Theme: https://github.com/PapirusDevelopmentTeam/adapta-kde
>
> Icons: https://github.com/PapirusDevelopmentTeam/papirus-icon-theme

### Prerequires:
- Install *kvantum-manjaro* using Octopi.
  - When extra dependencies are prompted, don't select any of them.
- Run the following command to install the *theme*
> wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/adapta-kde/master/install.sh | sh
- Run the following command to install the *icons*
> wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/papirus-icon-theme/master/install.sh | sh

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



