# problem:

```
1 - 
Does pressing fn key change the mode on your touchbar? 

uname -r
5.17.1-t2

cat /proc/cmdline
root=UUID=e6f0ac73-30bb-4326-9747-d2d15eae0c77 ro efi=noruntime pcie_ports=compat intel_iommu=on iommu=pt quiet splash vt.handoff=7 initrd=initrd.img-5.17.1-t2

sudo modprobe -r apple-ib-tb

ls -l /lib/systemd/system-sleep
total 12
-rwxr-xr-x 1 root root  92 Aug 21  2019 hdparm
-rwxr-xr-x 1 root root 133 Mar 29 16:22 rmmod_tb.sh
-rwxr-xr-x 1 root root 219 Jul 21  2020 unattended-upgrades

sudo modprobe -r apple-ib-tb

# we could simply remove the DKMS version cause the driver is built into the kernel 
sudo dkms uninstall -m apple-ibridge -v 0.1
sudo rm -r /usr/src/apple-bce*
sudo rm -r /usr/src/apple-ibridge*
sudo rm -r /var/lib/dkms/apple-bce
sudo rm -r /var/lib/dkms/apple-ibridge

sudo modprobe apple-ib-tb

sudo dmesg | grep apple-ib

dkms status


```

# problem "external display brightness"

```
# get the name of connected display which is not primary
xrandr -q | grep " connected"
eDP-1 connected primary 3408x2130+905+2880 (normal left inverted right x axis y axis) 286mm x 179mm
DP-3 connected 5120x2880+0+0 (normal left inverted right x axis y axis) 525mm x 295mm

# brightness can be set from 0-1
xrandr --output DP-3 --brightness 0.63
```

# problem "wifi"

```
# stop
sudo modprobe -r brcmfmac

# start
sudo modprobe brcmfmac

journalctl -k --grep=brcmfmac

sudo systemctl restart wpa_supplicant
sudo systemctl restart NetworkManager
```

# problem "bluetooth"

```
journalctl -k --grep=bluetooth
```

# other commands

```
# get the list of devices connected (internal/external)
lsusb

# backlights
ls /sys/class/leds
```

suspend script that unloads/reloads apple_ib_tb and thunderbolt
journalctl -k --grep=thunderbolt

# tail logs

    $ journalctl -kf

    -- Journalctl is a utility for querying and displaying logs from journald, systemd's logging service.

    journalctl -k --grep=apple-ib-touchbar

    usbhid.quirks=0x05ac:0x8302:0x80000 

    sudo dmesg | grep apple-ib

    journalctl -k --grep=URB

    -- T2 BCE/VHCI drivers which are needed for keyboard/trackpad/audio and aren't mainlined yet


# other

```py
import usb
dev=usb.core.find(idVendor=0x05ac, idProduct=0x8102)
dev.detach_kernel_driver(0)
dev.detach_kernel_driver(1)

pip install usb
sudo python
```

```py
» cat /lib/systemd/system-shutdown/unbind-tb.shutdown 
#!/usr/bin/env python3
try:
    import usb
    dev=usb.core.find(idVendor=0x05ac, idProduct=0x8102)
    dev.detach_kernel_driver(0)
    dev.detach_kernel_driver(1)
except Exception:
    print("we tried, oh well")

return 0
```

    dkms status

    ls -l /usr/src

```
sudo dkms uninstall -m apple-ibridge -v 0.1
sudo rm -r /usr/src/apple-ibridge-0.1
sudo rm -r /var/lib/dkms/apple-ibridge
sudo git clone https://github.com/AdityaGarg8/apple-ib-drv /usr/src/apple-ibridge-0.1
sudo dkms install -m apple-ibridge -v 0.1
```

Then edit /etc/modprobe.d/apple-tb.conf and add options apple-ib-tb keyboard_backlight=1 at the end
Then reboot