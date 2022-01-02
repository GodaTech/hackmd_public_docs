# Ubuntu Setup
###### tags: `Public`, `Technical`, `Configuration`, `Linux`, `Ubuntu`

## General Rule
- Hostname: `${PLATFORM}-${OS}-${'[[:digit:]][[:digit:]]'}` &rarr; `X79-Focal-00`
- Language: English

## Universal
### Auto Mount Disk
- Check target disk UUID
    - `sudo blkid /dev/sde1 `
```/dev/sde1: UUID="e030f70e-3006-468e-8ca9-1c95ce63e14c" TYPE="ext4" PARTUUID="5717dfe0-01"```
- Add command to `/etc/fstab`
```UUID=e030f70e-3006-468e-8ca9-1c95ce63e14c /mnt/uv500_raid0 ext4 defaults 0 2```

### Gig Config
- `git config --global user.email "user@gmail.com"`
- `git config --global user.name "User"`
- `git config --global core.editor vim`

### [Install Docker](https://hackmd.io/qtx67km3SsqvB6oxDlZVpA#Install-Docker-Engine-on-Ubuntu)

---

## Ubuntu 20.04.3 LTS (Focal Fossa)
- [64-bit PC (AMD64) desktop image](https://releases.ubuntu.com/focal/ubuntu-20.04.3-desktop-amd64.iso)
- Minimal Installation

### Disk Partitions
Mount the following path to **different partition**.
Recommended use **different physical disk** in virtual machine.
- `/` - at least 128 GB
    - EFI System Partition - 100 MB
    - swap area - 2 GB
- `/var` - at least 256 GB for Docker
- `/home` - at least 128 GB
- Total: 512GB or more

### Drivers

#### Hyper-V
- [Supported Ubuntu virtual machines on Hyper-V](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/supported-ubuntu-virtual-machines-on-hyper-v)
- `sudo apt install linux-azure`
- reboot

##### Change the Resolution of an Ubuntu Hyper-V Virtual Machine
- [x] Tested
- https://virtualizationreview.com/Blogs/Virtual-Insider/2014/09/change-ubuntu-resolution-on-hyper-v-vm.aspx
1. `sudo vim /etc/default/grub`
2. GRUB_CMDLINE_LINUX_DEFAULT="quiet splash" -> GRUB_CMDLINE_LINUX_DEFAULT="quiet splash video=hyperv_fb:1366x768"
3. `sudo update-grub`

##### Hyper-V Linux Guest VM Enhancements
- [ ] Tested
- [linux-vm-tools](https://github.com/z80020100/linux-vm-tools)
- [How to install Ubuntu 20.04 on Hyper-V with enhanced session](https://francescotonini.medium.com/how-to-install-ubuntu-20-04-on-hyper-v-with-enhanced-session-b20a269a5fa7)
- [Ubuntu 20.04 on Hyper-V](https://medium.com/@labappengineering/ubuntu-20-04-on-hyper-v-8888fe3ced64)

#### VMware
- VMware Tools
  - [x] Tested
  - [How to install VMware Tools on Ubuntu 20.04 LTS Linux using command line](https://www.how2shout.com/linux/how-to-install-vmware-tools-on-ubuntu-20-04-lts-linux-using-command-line/)
  - [安裝 Open VM Tools](https://docs.vmware.com/tw/VMware-Tools/11.3.0/com.vmware.vsphere.vmwaretools.doc/GUID-C48E1F14-240D-4DD1-8D4C-25B6EBE4BB0F.html)
  - `sudo apt install open-vm-tools-desktop`

#### NVIDIA

##### Install NVIDIA driver
- `ubuntu-drivers devices`

    ```
    WARNING:root:_pkg_get_support nvidia-driver-390: package has invalid Support Legacyheader, cannot determine support level
    == /sys/devices/pci0000:00/0000:00:03.0/0000:04:00.0 ==
    modalias : pci:v000010DEd00001C03sv00001462sd00003283bc03sc00i00
    vendor   : NVIDIA Corporation
    model    : GP106 [GeForce GTX 1060 6GB]
    driver   : nvidia-driver-390 - distro non-free
    driver   : nvidia-driver-418-server - distro non-free
    driver   : nvidia-driver-455 - third-party non-free
    driver   : nvidia-driver-470-server - distro non-free
    driver   : nvidia-driver-470 - third-party non-free recommended
    driver   : nvidia-driver-450 - third-party non-free
    driver   : nvidia-driver-450-server - distro non-free
    driver   : nvidia-driver-460-server - distro non-free
    driver   : nvidia-driver-460 - third-party non-free
    driver   : nvidia-driver-465 - third-party non-free
    driver   : xserver-xorg-video-nouveau - distro free builtin
    ```

- `sudo apt install nvidia-driver-470`

##### Remove all NVIDIA driver related packages
- `sudo apt purge nvidia*`

### Common
- `sudo apt update`
- `sudo apt install build-essential`
- Settings -> Region & Language -> Formats -> ~~United States~~ -> United Kingdom
- Settings -> Power -> Blank Screen -> Never
- `sudo apt install vim htop net-tools`

#### System Upgrade
- `sudo apt update`
- `sudo apt dist-upgrade`
- reboot
- `sudo apt clean`
- `sudo apt autoremove`

#### SSH
- `sudo apt install ssh` or `sudo apt install openssh-server`

#### Samba
- [Install and Configure Samba](https://ubuntu.com/tutorials/install-and-configure-samb)
- `sudo apt install samba`
- Add user for Samba
`sudo smbpasswd -a $USER`
- Setting up Samba
`sudo vim /etc/samba/smb.conf` 
```
[homes]
   comment = Home Directories
   path = /home/%S
   valid users = %S
   writable = yes
   browseable = yes
   create mask = 0644
   directory mask = 0755
```

#### Timeshift
- `sudo apt update`
- `sudo apt install timeshift`

#### Development Tools
- `sudo apt install git tig`

#### LAMP
- [How To Install Linux, Apache, MySQL, PHP (LAMP) stack on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-20-04)
- [How to Install a LAMP Stack on Ubuntu 20.04](https://www.linode.com/docs/guides/how-to-install-a-lamp-stack-on-ubuntu-20-04/)
##### Install Using Tasksel
- [x] Adopt
- `sudo apt update`
- `sudo apt install tasksel`
- `sudo tasksel install lamp-server`
- `sudo mysql_secure_installation`
##### Install Packages Separately
- Apache
  - `sudo apt update`
  - `sudo apt install apache2`
  - `sudo ufw app list`
  - `sudo ufw status`
- MySQL
  - `sudo apt install mysql-server`
  - `sudo mysql_secure_installation`
- PHP
  - `sudo apt install php libapache2-mod-php php-mysql`

#### Database managemen
##### phpMyAdmin
- [How To Install and Secure phpMyAdmin on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-20-04)
- Install phpMyAdmin
  - `sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl`
  - **For the server selection, choose `apache2`**
    - When the prompt appears, **apache2** is highlighted, but **not** selected
    - If you do not hit `SPACE` to select Apache, the installer will not move the necessary files during installation
    - Hit `SPACE`, `TAB`, and then `ENTER` to select Apache
  - Select Yes when asked whether to use dbconfig-common to set up the database
  - You will then be asked to choose and confirm a MySQL application password for phpMyAdmin
- Configuring password access for a dedicated MySQL user
  - ***NOTICE**: please change the **USER** and **PASSWORD** in the following command*
  - `sudo mysql`
  - `CREATE USER 'USER'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'PASSWORD';`
  - `GRANT ALL PRIVILEGES ON *.* TO 'USER'@'localhost' WITH GRANT OPTION;`
  - `exit`
##### Adminer
- [How to install Adminer on Ubuntu 20.04 LTS](https://danielfelix.in/blog/how-to-install-adminer-on-ubuntu-20-04-lts/)
- Installing Adminer
  - `sudo apt update`
  - `sudo apt install adminer`
- Configuring Adminer
  - `sudo a2enconf adminer`
  - `sudo systemctl reload apache2`

### Laptop

#### Disable suspend
- `sudo vim /etc/systemd/logind.conf`
- `HandleLidSwitch=ignore`

---

## Ubuntu 18.04.5 LTS (Bionic Beaver)
- [64-bit PC (AMD64) desktop image](https://releases.ubuntu.com/18.04.5/ubuntu-18.04.5-desktop-amd64.iso)
- Minimal Installation

### Common
- `sudo apt update`
- `sudo apt install build-essential`
- Settings -> Region & Language -> Formats -> ~~United States~~ -> United Kingdom
- Settings -> Power -> Blank Screen -> Never
- `sudo apt install vim htop net-tools`

#### System Upgrade
- `sudo apt upgrade`
- reboot
- `sudo apt autoclean`
- `sudo apt autoremove`

#### SSH
- `sudo apt install ssh` or `sudo apt install openssh-server`

#### Samba
- [Install and Configure Samba](https://ubuntu.com/tutorials/install-and-configure-samb)
- `sudo apt install samba`
- Add user for Samba
`sudo smbpasswd -a $USER`
- Setting up Samba
`sudo vim /etc/samba/smb.conf` 
```
[homes]
   comment = Home Directories
   path = /home/%S
   valid users = %S
   writable = yes
   browseable = yes
   create mask = 0644
   directory mask = 0755
```

### VMware
- VMware Tools
    - Add CD/DVD Drive
    - VM -> Install VMware Tools...
    - mkdir -p ~/vmware_tools
    - `cp /media/${USER}/VMware\ Tools/* ~/vmware_tools`
    - `cd ~/vmware_tools`
    - `tar -xvf VMwareTools-10.3.22-15902021.tar.gz`
    - `cd vmware-tools-distrib`
    - `sudo ./vmware-install.pl`

### NVIDIA Jetson Nano
- JetPack 4.6
- `sudo apt update`
- `sudo apt upgrade`
- System Settings -> Language Support -> Regional Formats -> English (United States)
- System Settings -> Brightness & Lock -> Turn screen off when inactive for: -> Never

### Macbook (Intel)
