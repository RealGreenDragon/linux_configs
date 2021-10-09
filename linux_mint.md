# Linux Mint Configuration Guide

Author: Daniele Giudice

Versions: from 20

## Set Italian Keyboard

#### Valid until reboot (CLI)
```
sudo loadkeys it
```

#### Valid until reboot (GUI)
```
sudo setxkbmap it
```

#### Permanent
```
sudo dpkg-reconfigure keyboard-configuration
```

## Bash proxy settings
```
export http_proxy=http://username:password@proxyhost:port/

export https_proxy=https://username:password@proxyhost:port/
```

## Install/Update VirtualBox Guest Additions + Enable Shared Folders Access

#### Scan repositories + Install 'VirtualBox Guest Additions' minimal dependencies + Update all installed packages
```
sudo apt-get -y update && sudo apt-get -y install build-essential dkms linux-headers-generic && sudo apt-get -y upgrade && sudo apt-get -y autoremove && sudo apt-get -y clean
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
sudo apt-get -y update && sudo apt-get -y upgrade && sudo apt-get -y install apt-transport-https build-essential ca-certificates curl gawk git hunspell-en-us hunspell-it linux-tools-common linux-tools-generic net-tools numlockx p7zip-full p7zip-rar perl python3 python3-doc python3-pip python3-venv sed software-properties-common unrar unzip vim wget zip && sudo apt-get -y autoremove && sudo apt-get -y clean
```

#### Python 3 virtualenv (local user, activated at startup)
```
python3 -m venv ~/py_env
source ~/py_env/bin/activate
echo -e "\nsource $HOME/py_env/bin/activate" >> ~/.bashrc
pip install --upgrade pip wheel setuptools testresources youtube_dl
```

### Audio/Video

#### Codecs + FFMpeg + RTMPDump
```
sudo apt-get -y install mint-meta-codecs ffmpeg rtmpdump
```

#### MediaInfo
```
sudo apt-get -y install mediainfo mediainfo-gui
```

#### MKVToolNix + GUI
```
wget -q -O - https://mkvtoolnix.download/gpg-pub-moritzbunkus.txt | sudo apt-key add - && echo "deb [arch=amd64 signed-by=/usr/share/keyrings/gpg-pub-moritzbunkus.gpg] https://mkvtoolnix.download/ubuntu/ $(grep 'PRETTY_NAME' /etc/os-release | awk '{ print $2 }') main" | sudo tee /etc/apt/sources.list.d/mkvtoolnix.download.list && sudo apt-get -y update && sudo apt-get -y install mkvtoolnix mkvtoolnix-gui
```

### Editors & IDE

#### Atom
```
wget -qO - https://packagecloud.io/AtomEditor/atom/gpgkey | sudo apt-key add - && sudo sh -c 'echo "deb [arch=amd64] https://packagecloud.io/AtomEditor/atom/any/ any main" > /etc/apt/sources.list.d/atom.list' && sudo apt-get -y update && sudo apt-get -y install atom
```

#### Notepadqq
```
sudo add-apt-repository -y ppa:notepadqq-team/notepadqq && sudo apt-get -y update && sudo apt-get -y install notepadqq
```

#### MiKTeX (local user installation, basic packages set, automatic package installation enabled, incompatible with TeXlive)
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys D6BC243565B2087BC3F897C9277A7293F59E4889 && echo "deb [arch=amd64] http://miktex.org/download/ubuntu $(grep 'PRETTY_NAME' /etc/os-release | awk '{ print $2 }') universe" | sudo tee /etc/apt/sources.list.d/miktex.list && sudo apt-get -y update && sudo apt-get -y install miktex && miktexsetup finish && initexmf --set-config-value [MPM]AutoInstall=1 && mpm --verbose --update && mpm --verbose --package-level=basic --upgrade
```

#### TeXstudio -> Requirements: MiKTeX
```
sudo add-apt-repository -y ppa:sunderme/texstudio && sudo apt-get -y update && sudo apt-get -y install texstudio
```

### Web & Programming

#### CMake
```
sudo apt-get -y install cmake
```

#### JDowloader 2 BETA (local user installation - no adware)
```
# Source JD2 Clean Installer:
# https://board.jdownloader.org/showthread.php?t=54725
# Source megadown
# https://github.com/tonikelope/megadown

sudo apt-get -y install wget curl pv jq
wget -O ~/megadown https://raw.githubusercontent.com/tonikelope/megadown/master/megadown
chmod +x ~/megadown
~/megadown -o ~/jd_setup_x64.sh 'https://mega.nz/#!LJ9FyK7b!t88t6YBo2Wm_ABkSO7GikxujDF5Hddng9bgDb8fwoJQ'
chmod +x jd_setup_x64.sh
~/jd_setup_x64.sh -q -dir ~/jd2 -overwrite &> jd_install_log.txt
rm -rf ~/megadown ~/.megadown/ ~/jd_setup_x64.sh ~/jd_install_log.txt ~/.oracle_jre_usage/
```

#### Tor Browser (local user installation) -> Requirements: python3
```
wget -O ~/tor.tar.xz "$(python3 -c "import urllib.request; d=urllib.request.urlopen('https://www.torproject.org/download/').read().decode('utf8').split('\">Download for Linux')[0].rsplit('\"', 1)[-1]; print('https://www.torproject.org'+d)")" && tar xJpf ~/tor.tar.xz -C ~/ && rm -f ~/tor.tar.xz && cd ~/tor-browser_en-US && ./start-tor-browser.desktop --register-app && cd $OLDPWD
```

#### XAMPP + Shortcut scripts in home directory -> Requirements: python3
```
wget -O ~/xampp-x64.run "$(python3 -c "import urllib.request; print(urllib.request.urlopen('https://www.apachefriends.org/it/index.html').read().decode('utf8').split('XAMPP per <strong>Linux')[0].rsplit('href=\"', 1)[-1].split('\"')[0])")" && chmod +x ~/xampp-x64.run && sudo ~/xampp-x64.run --mode unattended && rm -f ~/xampp-x64.run && sudo chmod o+rx -R /opt/lampp/htdocs/ && echo "/opt/lampp/manager-linux-x64.run" > ~/xampp_gui.sh && echo "/opt/lampp/lampp" > ~/xampp_service.sh && chmod a+x ~/xampp_*.sh
```

### Virtualization

#### Docker (current user access enabled) -> Reboot required
```
curl -fsSL https://get.docker.com -o ~/get-docker.sh && chmod a+x ~/get-docker.sh && sudo sh ~/get-docker.sh && rm -f ~/get-docker.sh && sudo usermod -aG docker $(whoami) && sudo reboot
```

### Network

#### Driver WiFi USB RTL8811CU/RTL8821CU -> Requirements: git + build-essential + dkms
```
mkdir -p ~/wifi
cd ~/wifi
git clone https://github.com/brektrou/rtl8821CU.git
cd rtl8821CU
chmod +x dkms-install.sh
sudo ./dkms-install.sh
sudo modprobe 8821cu
cd ~
rm -rf ~/wifi
```

#### Kathara + GUI -> Requirements: Docker + python 2.7 (aliased as 'python')
```
sudo apt-get -y install xterm python && sudo python -m pip install --upgrade ipaddress && sudo git clone --recursive https://github.com/KatharaFramework/Kathara.git /opt/kathara

tee -a ~/.bashrc > /dev/null <<EOT
# Kathara Env
export NETKIT_HOME=/opt/kathara/bin
export PATH=\$PATH:\$NETKIT_HOME
EOT

export NETKIT_HOME=/opt/kathara/bin

export PATH=$PATH:$NETKIT_HOME

$NETKIT_HOME/install
```

#### Nmap
```
sudo apt-get -y install nmap
```

#### Wireshark (current user access enabled) -> Reboot required
```
sudo apt-get -y install wireshark && sudo usermod -aG wireshark $(whoami) && sudo reboot
```

### File Sharing

#### aMule
```
sudo apt-get -y install amule
```

#### qBittorrent
```
sudo add-apt-repository -y ppa:qbittorrent-team/qbittorrent-stable && sudo apt-get -y update && sudo apt-get -y install qbittorrent
```