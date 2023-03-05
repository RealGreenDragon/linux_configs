# Ubuntu Configuration Guide

Author: Daniele Giudice

Versions: 9.x

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
sudo subscription-manager repos --enable codeready-builder-for-rhel-9-$(arch)-rpms
sudo dnf -y install yum-utils
sudo dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```

## Install/Update VirtualBox Guest Additions + Enable Shared Folders Access

#### Install 'VirtualBox Guest Additions' minimal dependencies
```
sudo dnf -y install kernel-devel-$(uname -r) kernel-headers-$(uname -r) gcc binutils make elfutils-libelf-devel dkms bzip2 perl
```

#### Mount 'Guest Additions CD' from menu "Devices->Insert Guest Additions CD image..." (if a window appear, click "Cancel" button)

#### Install/Update Guest Additions
```
sudo "$(df --output=target | grep VBox_GAs_)/VBoxLinuxAdditions.run"
```

#### Unmount 'Guest Additions CD'
```
sudo umount -f "$(df --output=target | grep VBox_GAs_)"
```

#### Remove 'Guest Additions CD' by right click on disk icon in VirtualBox window bottom-right and select "Remove disk from virtual drive"

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
sudo dnf -y install curl gawk git net-tools python3 python3-doc python3-pip python3-venv sed unrar unzip vim wget zip
sudo dnf -y update
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

#### Basic Python 3 Modules
```
sudo python3 -m pip install --upgrade pip wheel setuptools
```

### Audio/Video

#### FFMpeg
```
sudo dnf -y install ffmpeg
```

#### MediaInfo
```
sudo dnf -y install mediainfo
```

#### yt-dlp (via python3 pip)
```
sudo python3 -m pip install --upgrade yt-dlp
```
