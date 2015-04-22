# snappy-info

### Image Setup:
View getting started guide at https://developer.ubuntu.com/en/snappy/start/

In summy, to setup the appropriate tools for downloading images:
$ sudo -i
# add-apt-repository ppa:snappy-dev/beta
# apt-get update
# apt-get upgrade
# apt-get install snappy-tools bzr

To download a generic x86_64 image of size 3G use the following command:
# ubuntu-device-flash core -s 3 -o snappy_flash.img --enable-ssh --channel=ubuntu-core/devel --device=generic_amd64

See http://system-image.ubuntu.com/channels.json for other device definitions in the ubuntu-core/devel channel.

### Device Setup:
0. Boot (XUbuntu) LiveCD on target device
0. 
