```
$ sudo su

$ apt install dkms
$ dkms status

# apple-bce in the output is 0.1, then uninstall old one

$ dkms uninstall -m apple-bce -v 0.1
$ dkms uninstall -m apple-ibridge -v 0.1
$ rm -r /usr/src/apple-bce-0.1
$ rm -r /usr/src/apple-ibridge-0.1
$ rm -r /var/lib/dkms/apple-bce
$ rm -r /var/lib/dkms/apple-ibridge

# install v0.2

$ git clone https://github.com/t2linux/apple-bce-drv /usr/src/apple-bce-0.2
$ cd /usr/src/apple-bce-0.2
$ nano dkms.conf

    PACKAGE_NAME="apple-bce"
    PACKAGE_VERSION="0.2"
    MAKE[0]="make KVERSION=$kernelver"
    CLEAN="make clean"
    BUILT_MODULE_NAME[0]="apple-bce"
    DEST_MODULE_LOCATION[0]="/kernel/drivers/misc"
    AUTOINSTALL="yes"

$ dkms install -m apple-bce -v 0.2
```