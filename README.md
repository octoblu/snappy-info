# snappy-info
Light-weight devices are becoming more affordable and readily available. Similarly, Linux OSes such as Ubuntu Core are minimal, performant and accomodate the Internet of Things and other connected devices well.  
So let's load Ubuntu Core onto light-weight devices.

### Image Setup:
[Follow Snappy Ubuntu's getting started guide](https://developer.ubuntu.com/en/snappy/start/)

Set-up the appropriate tools for downloading images:
- `$ sudo -i`
- `add-apt-repository ppa:snappy-dev/beta`
- `apt-get update`
- `apt-get upgrade`
- `apt-get install snappy-tools bzr`

To download a generic x86_64 image of size 3.9GB, use the following command:  
`sudo ubuntu-device-flash core 15.04 -s 4 -o snappy.img --enable-ssh --channel=stable --device=generic_amd64`

To download device/architecture-specific images, check out the [device definitions](http://system-image.ubuntu.com/channels.json) in the ubuntu-core/devel channel.

#### Target Device Setup:
0. Boot (XUbuntu) LiveUSB on target device and select desktop preview
0. Prepare netcat to write to device: `nc -l 9999 | sudo dd of=/dev/mmcblk0 bs=32M` (where output file is mmcblkX or sdX)

#### Wired Host Setup:
0. From an Ubuntu host make sure under ethernet IPv4 settings that 'Method' is set to 'Share to other computers'
0. Connect ethernet from target device to host
0. Make sure bar is installed: `sudo apt-get install bar`
0. Copy the snappy image over the network to host device: `bar snappy.img | nc 10.42.0.22 9999`

#### Target Partition Setup:
0. Boot to LiveUSB (or reboot if already in LiveUSB) and select desktop preview
0. Use `sude gparted` from a terminal to resize partitions, with at least 1.5GB for 'system-a'
0. Remove LiveUSB and reboot device

#### Apt setup from host:
0. Download most current apt package from host (https://launchpad.net/ubuntu/vivid/amd64/apt/)
0. Copy package to target device: `scp apt_X.X.X.Xubuntu4_amd64.deb ubuntu@10.42.0.22:apt.deb`

#### Apt setup from target:
0. Remount root filesystem as writable: `sudo mount -o remount,rw /`
0. Install desired package: `dpkg -i apt.deb`
0. Remove no-apt links: `cd /usr/local/bin; sudo rm apt*`
0. Update apt: `sudo apt-get update`

### Graphical Setup:

[More info on low memory setups](https://help.ubuntu.com/community/Installation/LowMemorySystems)

1. Install xorg and wget on target device: `sudo apt-get install -y xorg wget`
1. Download chrome: `wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb -O chrome.deb`
1. Install chrome.deb to target: `dpkg -i chrome.deb`
1. Install chrome dependencies: `sudo apt-get -f install`
1. To setup chrome to launch without a window manager place the following in ~/.xinitrc: `/opt/google/chrome/google-chrome --app=https://web.com/`
1. Create the following file at `/etc/systemd/system/xinit@.service`: 
    ```
    [Unit]
    Description=startx for user %i
    After=x@vt7.service systemd-user-sessions.service
    Wants=x@vt7.service
    Conflicts=getty@tty7.service
    
    [Service]
    User=%i
    TTYPath=/dev/tty7
    PAMName=login
    Environment=DISPLAY=:0
    WorkingDirectory=/home/%I
    Nice=0
    ExecStart=/bin/bash -l -c "while true; do startx -- :0 vt7 >/dev/null 2>&1; done"
    
    [Install]
    WantedBy=graphical.target
    ```
1. Enable xinit for the ubuntu user: `sudo systemctl enable xinit@ubuntu`
1. Enable x11-common for 'Anybody': `sudo dpkg-reconfigure x11-common`
