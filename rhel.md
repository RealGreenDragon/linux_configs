# RHEL/CentOS Configuration Guide

Author: Daniele Giudice

Versions: 8.x, 9.x

## Set Italian Keyboard

```
sudo localectl set-keymap it
```

## Bash proxy settings
```
export http_proxy=http://username:password@proxyhost:port/

export https_proxy=https://username:password@proxyhost:port/
```

## Enable EPEL repository
```
export OS_MAJOR=$(cat /etc/os-release | grep '^VERSION=' | cut -d'=' -f2 | tr -d '"')
sudo subscription-manager repos --enable codeready-builder-for-rhel-${OS_MAJOR}-$(arch)-rpms
sudo dnf -y install yum-utils redhat-lsb-core
sudo dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-${OS_MAJOR}.noarch.rpm
```

## Install/Update VirtualBox Guest Additions + Enable Shared Folders Access

#### Install 'VirtualBox Guest Additions' minimal dependencies
```
sudo dnf -y install kernel-devel-$(uname -r) kernel-headers-$(uname -r) gcc binutils make elfutils-libelf-devel dkms bzip2 perl
sudo dnf -y install xorg-x11-xauth xorg-x11-fonts-* xorg-x11-utils dbus-x11 # extra dependencies for X11 Forwarding
sudo dnf -y install xterm # to test X11 Forwarding
```

#### Mount 'Guest Additions CD' from menu "Devices->Insert Guest Additions CD image..." (if a window appear, click "Cancel" button)

#### Install/Update Guest Additions
```
sudo mkdir -p /media/cdrom
sudo mount -o ro /dev/cdrom /media/cdrom
sudo /media/cdrom/VBoxLinuxAdditions.run --nox11 # remove "nox11" option if GUI setup
```

#### Unmount and eject 'Guest Additions CD'
```
sudo umount -f /media/cdrom
eject /dev/cdrom

```

#### Add current user in 'vboxsf' group to grant access to shared folders
```
sudo usermod -aG vboxsf $(whoami)
```

#### Reboot to enable changes
```
sudo reboot
```

## Software (if not specified the latest available x64 en-us version will be installed, and the installation is system-wide)

### General

#### Base software (python2 is NOT installed)
```
sudo dnf -y update && sudo dnf -y install curl gawk git less net-tools python3 python3-pip python3-virtualenv sed unrar unzip vim wget zip
```

#### Python 3 virtualenv (local user, activated at startup)
```
# Mandatory
python3 -m venv ~/py_env
source ~/py_env/bin/activate
echo -e "\nsource $HOME/py_env/bin/activate" >> ~/.bashrc
pip install --upgrade pip wheel setuptools
# Optional
pip install --upgrade yt-dlp
```

### Audio/Video

#### FFMpeg
```
sudo dnf -y install ffmpeg-free
```

#### MediaInfo
```
sudo dnf -y install mediainfo
```

#### yt-dlp (via python3 pip)
```
sudo python3 -m pip install --upgrade yt-dlp
```
