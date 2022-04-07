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
xrandr -q | grep " connected"
eDP-1 connected primary 3408x2130+905+2880 (normal left inverted right x axis y axis) 286mm x 179mm
DP-3 connected 5120x2880+0+0 (normal left inverted right x axis y axis) 525mm x 295mm

# brightness can be set from 0-1
xrandr --output DP-3 --brightness 0.63
```

# problem "wifi"

```
sudo modprobe -r bcm5974
```