# Fedora 44 - Gnome Setup

## Pre-Requesites

1. Fedora Workstation: [Download Fedora Workstation](https://fedoraproject.org/workstation/download/)
2. Bootable media: If using Ventoy, just copy the downloaded ISO to the flash drive, or use the Fedora    Media Writer from the fedora site to create a bootable flash drive. 
3. Backup existing data, although Fedora can be installed alongside Windows or any other OS, managing the partitions is a pain and I prefer using the whole drive for a single OS and this process will wipe the drive.
4. Device drivers, Unlike Windows, Fedora sometimes cannot find storage drivers for some SSD's, so it is a good idea to put the driver on the same drive so it can be loaded if required.

## Installation

Just follow the install process, we will not go into much detail here as the process changes often based on the OS, DE and the device where we are installing.

## Setup

### DNF Configuration

We will set the max paralled downloads and also set up dnf to default to Yes and to keep downloaded packages in cache to speed up reinstalls.

Open the dnf config in Nano

```bash
sudo nano /etc/dnf/dnf.conf
```

Paste the following under the [main] section

```bash
max_parallel_downloads=10
defaultyes=True
keepcache=True
```

### Update and Reboot

It is a good idea to update and reboot the system at this point.

```bash
sudo dnf -y update
```

```bash
sudo reboot
```

### RPM Fusion

RPM Fusion adds missing free and proprietary packages to fedora (media codecs, NVIDIA drivers etc)

```bash
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

Swap the included ffmpeg-free with the RPM Fusion version

```bash
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
```

Upgrade multimedia for hardware acceleration works

```bash
sudo dnf group upgrade multimedia
```

Also upgrade the Core so Software Center sees all apps

```bash
sudo dnf group upgrade core
```

### Firmware Updates

Although this does not seem to work for Acer Laptops, it is worth running just in case new drivers are available.

```bash
fwupdmgr refresh --force
fwupdmgr get-devices
fwupdmgr get-updates
fwupdmgr update
```

### NVIDIA Drivers

RPM fusion provides the proprietary NVIDIA drivers for Fedora via akmod

```bash
sudo dnf install akmod-nvidia
```

CUDA suport can be enabled with

```bash
sudo dnf install xorg-x11-drv-nvidia-cuda
```

### Flatpak

Enabling Third Party Repositories adds this automatically, but can be added manually in case we forget to select it.

```bash
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

### Tailscale

Next step is to install Tailscale with the Linux install script.

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

Connect with

```bash
sudo tailscale up
```

### Github CLI

Install

```bash
sudo dnf install dnf5-plugins
sudo dnf config-manager addrepo --from-repofile=https://cli.github.com/packages/rpm/gh-cli.repo
sudo dnf install gh
```

Upgrade

```bash
sudo dnf update gh
```