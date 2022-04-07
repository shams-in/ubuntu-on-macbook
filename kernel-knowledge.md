# (DKMS) Dynamic Kernel Module Support

Kernel modules (aka. DKMS) are additional pieces of software (out-of-tree) capable of being inserted into the Linux kernel to add functionality, such as a hardware driver.

Kernel modules automatically rebuilts when a new kernel is installed.

Kernel module file extension ".ko"

Example: https://github.com/AdityaGarg8/apple-ib-drv

    $ dkms install -m apple-ibridge -v 0.1

```conf dkms.conf
PACKAGE_NAME="apple-ibridge"
PACKAGE_VERSION="0.1"
CLEAN="make clean"
MAKE="make"
BUILT_MODULE_NAME[0]="apple-ibridge"
BUILT_MODULE_NAME[1]="apple-ib-tb"
BUILT_MODULE_NAME[2]="apple-ib-als"
DEST_MODULE_LOCATION[0]="/updates"
DEST_MODULE_LOCATION[1]="/updates"
DEST_MODULE_LOCATION[2]="/updates"
AUTOINSTALL="yes"
REMAKE_INITRD="yes"
```

    modprobe apple-ib-tb
    modprobe apple-ib-als

linux tool: 

    $ sudo apt install dkms
    $ dkms status

# modprobe

is a Linux program used to add a loadable kernel module to the Linux kernel or to remove a loadable kernel module from the kernel.

Eg:

    modprobe apple-ib-tb (add)
    modprobe -r apple-ib-tb (remove)

Config to pass params to kernel modules

    /etc/modprobe.d/apple-tb.conf

# lsmod

display currently loaded modules

also used to display the status of modules in the Linux kernel.

    lsmod | grep kvm
    lsmod | grep apple

# modinfo

gives detailed information about the kernel module

# dmesg (diagnostic messages) 

dmesg is a command on most Unix-like operating systems that prints the message buffer of the kernel. 

The output includes messages produced by the device drivers.

Eg:

    sudo dmesg | grep brcmfmac

# journalctl

Journalctl is a utility for querying and displaying logs from journald, systemd's logging service

Eg:

    journalctl -k --grep=brcmfmac
    journalctl -kf (tail)

# journalctl vs dmesg

Dmesg : a command that prints kernel logs.
Journald: a service that collects logs from systemd.

# kernel version

    uname -r

# kernel boot arguments

    cat /proc/cmdline

Eg:

    root=UUID=e6f0ac73-30bb-4326-9747-d2d15eae0c77 ro efi=noruntime pcie_ports=compat intel_iommu=on iommu=pt quiet splash vt.handoff=7 initrd=initrd.img-5.17.1-t2

# lsusb

lsusb is a utility for displaying information about USB buses in the system and the devices connected to them. 

    lsusb -t
    lsusb

# lshw

The “lshw” command is a small tool to display a complete picture of hardware configuration.

    lshw
    lshw -short

# links

https://linuxhint.com/100_essential_linux_commands/
https://linuxhint.com/linux-commands-guide/

# hciconfig

hciconfig is used to configure Bluetooth devices. 

hciX is the name of a Bluetooth device installed in the system.

    sudo hciconfig hci0 sspmode 1
    sudo hciconfig hci0 down
    sudo hciconfig hci0 up

# xrandr

Xrandr is used to set the size, orientation and/or reflection of the outputs for a screen.

    xrandr -q | grep " connected"
    xrandr --output DP-3 --brightness 0.63

# /sys/class

The directory `/sys/class` is exported by the kernel at run time, exposing the hierarchy of the hardware through sysfs.

Eg:

    cat /sys/class/dmi/id/product_name (has macbook product name)
    echo 60 | sudo tee /sys/class/leds/apple::kbd_backlight/brightness (change keyboard brightness)

# /dev

The files in `/dev` are actual devices files which `udev` creates at run time.

# udev

https://opensource.com/article/18/11/udev


