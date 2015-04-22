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

To download a generic x86_64 image of size 3G, use the following command:  
`ubuntu-device-flash core -s 3 -o snappy_flash.img --enable-ssh --channel=ubuntu-core/devel --device=generic_amd64`

For Ubuntu Core device/architecture-specific images, check out the [device definitions](http://system-image.ubuntu.com/channels.json) in the ubuntu-core/devel channel.

### Device Setup:
0. Boot (XUbuntu) LiveCD on target device


### Graphical Setup:

[More info on low memory setups](https://help.ubuntu.com/community/Installation/LowMemorySystems)

To setup chrome without a window manager, include the following in ~/.xinitrc:  
`/opt/google/chrome/google-chrome --app=https://web.com/`

