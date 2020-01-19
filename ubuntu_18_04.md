# Ubuntu 18.04 x64 Configuration Guide

Author: Daniele Giudice

## Set Italian Keyboard

#### Valid until reboot (CLI)
sudo loadkeys it

#### Valid until reboot (GUI)
sudo setxkbmap it

#### Permanent
sudo dpkg-reconfigure keyboard-configuration

## Bash proxy settings

export HTTP_PROXY=http://username:password@proxyhost:port/

export HTTPS_PROXY=https://username:password@proxyhost:port/

## Enable all base repositories
sudo add-apt-repository -y main && sudo add-apt-repository -y universe && sudo add-apt-repository -y restricted && sudo add-apt-repository -y multiverse && sudo apt -y update

## Install/Update VirtualBox Guest Additions + Enable Shared Folders Access

#### Update all packages + Install 'VirtualBox Guest Additions' minimal dependencies
sudo apt -y update && sudo apt -y upgrade && sudo apt -y install build-essential virtualbox-guest-dkms linux-headers-virtual && sudo apt -y autoremove && sudo apt -y clean

#### Mount 'Guest Additions CD' from menu "Devices->Insert Guest Additions CD image..."

#### Install/Update Guest Additions
sudo "$(df --output=target | grep VBox_GAs_)/VBoxLinuxAdditions.run"

#### Unmount 'Guest Additions CD'
sudo umount -f "$(df --output=target | grep VBox_GAs_)"

#### Remove 'Guest Additions CD' by right click on disk icon in VirtualBox window bottom-right and select "Remove disk from virtual drive"

#### Add current user in 'vboxsf' group to grant access to shared folders
sudo usermod -aG vboxsf $(whoami)

#### Reboot to enable changes
sudo reboot

## Software (if not specified, latest available version will be installed)

#### Base software
