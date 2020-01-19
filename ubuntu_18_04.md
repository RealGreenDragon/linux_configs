# Ubuntu 18.04 LTS Configuration Guide

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
sudo apt -y update && sudo apt -y upgrade && sudo apt -y install apt-transport-https build-essential curl wget net-tools unrar zip p7zip-full p7zip-rar sed cut gawk vim git qpdf python3 python3-dev python3-doc python3-pip python3-venv && sudo apt -y autoremove && sudo apt -y clean

#### Codecs + VLC + FFMpeg + Mediainfo
sudo apt -y update && sudo apt -y upgrade && sudo apt -y install vlc vlc-data libdvdnav4 libdvdread4 gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly libdvd-pkg ubuntu-restricted-extras ffmpeg rtmpdump ffmpegthumbnailer mediainfo mediainfo-gui && sudo apt -y autoremove && sudo apt -y clean

#### Pyhton3 Modules + youtube_dl
sudo python3 -m pip install --upgrade pip wheel setuptools requests python_utils pycryptodome youtube_dl

#### MiKTeX (system-wide, basic installation, automatic package installation enabled)
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys D6BC243565B2087BC3F897C9277A7293F59E4889 && echo "deb http://miktex.org/download/ubuntu $(lsb_release -cs) universe" | sudo tee /etc/apt/sources.list.d/miktex.list && sudo apt -y update && sudo apt -y install miktex && sudo miktexsetup --shared=yes finish && sudo initexmf --admin --set-config-value [MPM]AutoInstall=1 && sudo mpm --admin --verbose --package-level=basic --upgrade && sudo apt -y autoremove && sudo apt -y clean

#### MKVToolNix + GUI
wget -q -O - https://mkvtoolnix.download/gpg-pub-moritzbunkus.txt | sudo apt-key add - && echo "deb https://mkvtoolnix.download/ubuntu/ $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/mkvtoolnix.download.list && sudo apt -y update && sudo apt -y install mkvtoolnix mkvtoolnix-gui && sudo apt -y autoremove && sudo apt -y clean

#### Notepadqq
sudo add-apt-repository -y ppa:notepadqq-team/notepadqq && sudo apt -y update && sudo apt install -y notepadqq && sudo apt -y autoremove && sudo apt -y clean

#### Atom
wget -O ~/atom-amd64.deb https://atom.io/download/deb && sudo apt -y install ~/atom-amd64.deb && rm -f ~/atom-amd64.deb && sudo apt -y autoremove && sudo apt -y clean

#### XAMPP v7.4.1 x64 + Shortcut scripts in home directory
wget -O ~/xampp-x64.run "https://www.apachefriends.org/xampp-files/7.4.1/xampp-linux-x64-7.4.1-0-installer.run" && chmod a+x ~/xampp-x64.run && sudo ~/xampp-x64.run --mode unattended && rm -f ~/xampp-x64.run && sudo chmod o+rx -R /opt/lampp/htdocs/ && echo "/opt/lampp/manager-linux-x64.run" > ~/xampp_gui.sh && echo "/opt/lampp/lampp" > ~/xampp_service.sh && chmod a+x ~/xampp_*.sh

#### Docker (current user access enabled) -> Reboot required
curl -fsSL https://get.docker.com -o ~/get-docker.sh && chmod a+x ~/get-docker.sh && sudo sh ~/get-docker.sh && rm -f ~/get-docker.sh && sudo usermod -aG docker $(whoami) && sudo reboot

#### Wireshark (current user access enabled) -> Reboot required
sudo apt -y install wireshark && sudo usermod -aG wireshark $(whoami) && sudo reboot

#### Nmap
sudo apt -y install nmap && sudo apt -y autoremove && sudo apt -y clean

#### Kathara + GUI -> Requirements: Docker + python 2.7 (aliased as 'python')
sudo apt -y install xterm python && sudo python -m pip install --upgrade ipaddress && sudo git clone --recursive https://github.com/KatharaFramework/Kathara.git /opt/kathara

printf "\n#Kathara Config\nexport NETKIT_HOME=/opt/kathara/bin\nexport PATH=\$PATH:\$NETKIT_HOME" >> ~/.bashrc

export NETKIT_HOME=/opt/kathara/bin

export PATH=$PATH:$NETKIT_HOME

$NETKIT_HOME/install

#### qBittorrent
sudo add-apt-repository -y ppa:qbittorrent-team/qbittorrent-stable && sudo apt -y update && sudo apt install -y qbittorrent && sudo apt -y autoremove && sudo apt -y clean

#### aMule
sudo apt install -y amule && sudo apt -y autoremove && sudo apt -y clean
