https://wiki.t2linux.org/

https://github.com/marcosfad/mbp-ubuntu
https://github.com/t2linux/wiki/blob/a4b46a7cfbe7efcbb6a0b6111e22172b0f5c4a77/docs/guides/wifi.md

https://gist.github.com/stephen-huan/dfec407ea31707f1ef43c1c7e1d10733

```
$ ioreg -l | grep RequestedFiles
C-4364s-B2/kauai.trx (ekans.trx)
C-4364s-B2/kauai-X3.clmb (kauai.clmb)
C-4364s-B2/P-kauai-X3_M-HRPN_V-u__m-7.5.txt (P-kauai_M-HRPN_V-u__m-7.5.txt)

cd /usr/share/firmware/wifi
right click > select "Show Original" to get the actual files
```

https://wiki.t2linux.org/guides/wifi/

For Ubuntu, the "t2-j-bigsur", "t2-hwe-bigsur" and "t2-big-sur" variant kernels use Big Sur firmware too. 
You can get them from here or this alternative link (https://github.com/t2linux/T2-Ubuntu-Kernel#pre-installation-steps). 
If you are a 16,1 or 16,2 user and want support for both bluetooth and wifi, then you will have to install a kernel available on the alternative link given above. 
Make sure that your DKMS Modules are updated and you install linux-headers before linux-image.

```
sudo sh wifi.sh
Password:
Detected macOS
Mounting the EFI partition
Volume EFI on disk0s1 mounted
Getting Wi-Fi firmware
Copying this script to EFI
Unmounting the EFI partition
Volume EFI on disk0s1 unmounted

-e Run the following commands or run this script itself in Linux now to set up Wi-Fi :-

sudo umount /dev/nvme0n1p1
sudo mkdir /tmp/apple-wifi-efi
sudo mount /dev/nvme0n1p1 /tmp/apple-wifi-efi
bash /tmp/apple-wifi-efi/wifi.sh
```

Product name: MacBookPro16,2

Wifi chipset number: e5:00.0 Network controller: Broadcom Inc. and subsidiaries BCM4364 802.11ac Wireless Network Adapter (rev 04)

MacBookPro16,2 	BCM4364 	4 	Trinidad 	Big Sur

audio drivers
https://gist.github.com/MCMrARM/c357291e4e5c18894bea10665dcebff

Debug wifi: (wifi adapter not found)
$ journalctl -k --grep=brcm > journal.txt

https://github.com/peterzxli/ubuntu_t2mac

ls -l /lib/firmware/brcm | grep 4364

-- dynamically reload the driver
sudo modprobe -r brcmfmac
sudo modprobe brcmfmac

sudo dmesg | grep brcmfmac

brcm/brcmfmac4364b3-pcie
brcm/brcmfmac4364b3-pcie.apple,trinidad-HRPN-u-7.7-X0.bin
brcm/brcmfmac4364b3-pcie.apple,trinidad-HRPN-u-7.7.bin


shams@shams-ubuntu:/lib/firmware/brcm$ sudo modprobe -r brcmfmac
shams@shams-ubuntu:/lib/firmware/brcm$ sudo modprobe brcmfmac
shams@shams-ubuntu:/lib/firmware/brcm$ sudo dmesg | grep -i brcm

[ 3356.814291] usbcore: deregistering interface driver brcmfmac
[ 3363.349552] usbcore: registered new interface driver brcmfmac
[ 3363.464549] brcmfmac: brcmf_fw_alloc_request: using brcm/brcmfmac4364b3-pcie for chip BCM4364/4
[ 3363.464613] brcmfmac 0000:e5:00.0: Direct firmware load for brcm/brcmfmac4364b3-pcie.apple,trinidad-HRPN-u-7.7-X0.bin failed with error -2
[ 3363.464642] brcmfmac 0000:e5:00.0: Direct firmware load for brcm/brcmfmac4364b3-pcie.apple,trinidad-HRPN-u-7.7.bin failed with error -2
[ 3363.464663] brcmfmac 0000:e5:00.0: Direct firmware load for brcm/brcmfmac4364b3-pcie.apple,trinidad-HRPN-u.bin failed with error -2
[ 3363.464686] brcmfmac 0000:e5:00.0: Direct firmware load for brcm/brcmfmac4364b3-pcie.apple,trinidad-HRPN.bin failed with error -2
[ 3363.464710] brcmfmac 0000:e5:00.0: Direct firmware load for brcm/brcmfmac4364b3-pcie.apple,trinidad-X0.bin failed with error -2
[ 3364.171295] brcmfmac: brcmf_c_process_txcap_blob: TxCap blob found, loading
[ 3364.173270] brcmfmac: brcmf_c_preinit_dcmds: Firmware: BCM4364/4 wl0: Jul 12 2021 18:02:56 version 9.30.464.0.32.5.76 FWID 01-c081cfed

# command and control key swap

```
~/.Xmodmap


```


## wifi fix (final) 

# imp: no need to copy files from mac

```
For ubuntu users facing issue with connecting to wifi

edit /etc/NetworkManager/NetworkManager.conf to look like this

[main]
plugins=ifupdown,keyfile

[ifupdown]
managed=false

[device]
wifi.scan-rand-mac-address=no

and edit /etc/NetworkManager/conf.d/wifi_backend.conf to look like this

#[device]
#wifi.backend=iwd

Then run
sudo systemctl restart NetworkManager
```


# touch 

```
$ modinfo apple-ib-tb

If the module is not found, try updating [apple-ibridge](https://wiki.t2linux.org/guides/dkms/)

$ lsusb

ID 05ac:8102 Apple, Inc. Touch Bar Backlight
ID 05ac:8302 Apple, Inc. Touch Bar Display

If they are not present, reboot without force shutting down, and see if they now are present. 
They are known to not show up for the first boot after the T2 chip restarts (which happens when you reset the SMC or force shutdown your Mac).

If it still isn't working, check `journalctl -k --grep="touch bar mode"`. 
Look for this line (the 2 could be a different number):

kernel: apple-ib-touchbar 0003:05AC:8102.000C: tb: Failed to set touch bar mode to 2 (-110)

 If it's present, reboot without any external usb devices plugged in, or update your kernel
    

## after its working

set keyboard backlight
$ modprobe apple-ib-tb
$ modprobe apple-ib-als
$ options apple-ib-tb fnmode=2
$ echo 20 | sudo tee /sys/class/leds/apple::kbd_backlight/brightness
```

## apply thunderbolt display

    $ lspci -vt

logs after connecting

```
shams@shams-ubuntu:~$ journalctl -f
Apr 03 08:26:30 shams-ubuntu kernel: thunderbolt 0-1: new device found, vendor=0x1e device=0x1115
Apr 03 08:26:30 shams-ubuntu kernel: thunderbolt 0-1: LG Electronics UltraFine 4K
Apr 03 08:26:30 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] parent is 97fc0400-0000...
Apr 03 08:26:30 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] connected: connected (/sys/devices/pci0000:00/0000:00:0d.2/domain0/0-0/0-1)
Apr 03 08:26:30 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] auto-auth: authmode: enabled, policy: iommu, iommu: yes -> ok
Apr 03 08:26:30 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] auto-auth: security: iommu+user mode, key: no -> ok
Apr 03 08:26:30 shams-ubuntu boltd[714]: probing: started [1000]
Apr 03 08:26:30 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] authorize: authorization prepared for 'user' level
Apr 03 08:26:30 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] udev: device changed: authorizing -> authorizing
Apr 03 08:26:31 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] udev: device changed: authorizing -> authorizing
Apr 03 08:26:31 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] authorize: finished: ok (status: authorized, flags: 0)
Apr 03 08:26:31 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] auto-auth: authorization successful
Apr 03 08:26:31 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] udev: device changed: authorized -> authorized

shams@shams-ubuntu:~$ boltctl list
 ○ LG Electronics UltraFine 4K
   ├─ type:          peripheral
   ├─ name:          UltraFine 4K
   ├─ vendor:        LG Electronics
   ├─ uuid:          c5030000-0072-740e-83ba-b184dc03e101
   ├─ status:        disconnected
   ├─ authorized:    Sat 02 Apr 2022 12:36:08 PM
   ├─ connected:     Sat 02 Apr 2022 12:36:08 PM
   └─ stored:        Sat 02 Apr 2022 12:14:31 PM
      ├─ policy:     iommu
      └─ key:        no

 ● LG Electronics UltraFine 4K #2
   ├─ type:          peripheral
   ├─ name:          UltraFine 4K
   ├─ vendor:        LG Electronics
   ├─ uuid:          bd030000-0082-840e-033a-219308623003
   ├─ status:        authorized
   │  ├─ domain:     97fc0400-0000-0000-0000-000000000000
   │  └─ authflags:  none
   ├─ authorized:    Sun 03 Apr 2022 12:26:31 PM
   ├─ connected:     Sun 03 Apr 2022 12:26:30 PM
   └─ stored:        Sat 02 Apr 2022 12:14:31 PM
      ├─ policy:     iommu
      └─ key:        no

# take note of the uuid of your device
boltctl authorize XXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX

shams@shams-ubuntu:~$ boltctl authorize bd030000-0082-840e-033a-219308623003

shams@shams-ubuntu:~$ boltctl enroll bd030000-0082-840e-033a-219308623003
device with id 'bd030000-0082-840e-033a-219308623003' already enrolled.

shams@shams-ubuntu:~$ journalctl -u bolt.service -f
-- Logs begin at Sat 2022-04-02 07:58:47 EDT. --
Apr 03 08:42:22 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] auto-auth: security: iommu+user mode, key: no -> ok
Apr 03 08:42:22 shams-ubuntu boltd[714]: probing: started [1000]
Apr 03 08:42:22 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] authorize: authorization prepared for 'user' level
Apr 03 08:42:22 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] udev: device changed: authorizing -> authorizing
Apr 03 08:42:22 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] udev: device changed: authorizing -> authorizing
Apr 03 08:42:22 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] authorize: finished: ok (status: authorized, flags: 0)
Apr 03 08:42:22 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] auto-auth: authorization successful
Apr 03 08:42:22 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] udev: device changed: authorized -> authorized
Apr 03 08:42:25 shams-ubuntu boltd[714]: probing: timeout, done: [2995901] (2000000)
Apr 03 08:43:00 shams-ubuntu boltd[714]: [bd030000-0082-UltraFine 4K               ] enroll: got 'default' policy, adjusted to: 'iommu'
```

https://blog.wains.be/2022/2022-02-10-thunderbolt-security-management/
https://superuser.com/questions/1210310/thunderbolt-display-not-recognised-with-ubuntu-16-04
https://moebiuscurve.wordpress.com/2014/10/11/configuring-3rd-monitor-to-my-alienware-m17xr4-ubuntu14-04-desktop/

$ lsusb -t
$ lshw
```
*-pci:0
             description: PCI bridge
             product: Ice Lake Thunderbolt 3 PCI Express Root Port #0
             vendor: Intel Corporation
             physical id: 7
             bus info: pci@0000:00:07.0
             version: 03
             width: 32 bits
             clock: 33MHz
             capabilities: pci normal_decode bus_master cap_list
             resources: ioport:4000(size=8192) memory:81600000-885fffff ioport:b1300000(size=117440512)
        *-pci:1
             description: PCI bridge
             product: Ice Lake Thunderbolt 3 PCI Express Root Port #1
             vendor: Intel Corporation
             physical id: 7.1
             bus info: pci@0000:00:07.1
             version: 03
             width: 32 bits
             clock: 33MHz
             capabilities: pci normal_decode bus_master cap_list
             resources: ioport:6000(size=8192) memory:88600000-8f5fffff ioport:b8300000(size=117440512)
        *-pci:2
             description: PCI bridge
             product: Ice Lake Thunderbolt 3 PCI Express Root Port #2
             vendor: Intel Corporation
             physical id: 7.2
             bus info: pci@0000:00:07.2
             version: 03
             width: 32 bits
             clock: 33MHz
             capabilities: pci normal_decode bus_master cap_list
             resources: ioport:8000(size=8192) memory:8f600000-965fffff ioport:bf300000(size=117440512)
        *-pci:3
             description: PCI bridge
             product: Ice Lake Thunderbolt 3 PCI Express Root Port #3
             vendor: Intel Corporation
             physical id: 7.3
             bus info: pci@0000:00:07.3
             version: 03
             width: 32 bits
             clock: 33MHz
             capabilities: pci normal_decode bus_master cap_list
             resources: ioport:a000(size=8192) memory:96600000-9d5fffff ioport:c6300000(size=117440512)

```
```
# check if iommu is enabled (if it doesn't return anything then follow the below step to enable it)
$ sudo dmesg | grep -i "iommu"

# enable iommu
$ sudo nano /etc/default/grub (append the old values)

    GRUB_CMDLINE_LINUX_DEFAULT="XXX intel_iommu=on iommu=pt"

$ sudo update-grub
$ sudo reboot

## display is working (but have to connect the display while it is rebooting)

shams@shams-ubuntu:~$ sudo xrandr -q
[sudo] password for shams: 
Screen 0: minimum 320 x 200, current 8768 x 4066, maximum 16384 x 16384
eDP-1 connected primary 2560x1600+2714+2466 (normal left inverted right x axis y axis) 286mm x 179mm
   2560x1600     60.00*+  59.99    59.97  
   2560x1440     59.99    59.99    59.96    59.95  
   2048x1536     60.00  
   1920x1440     60.00  
   1856x1392     60.01  
   1792x1344     60.01  
   2048x1152     59.99    59.98    59.90    59.91  
   1920x1200     59.88    59.95  
   1920x1080     60.01    59.97    59.96    59.93  
   1600x1200     60.00  
   1680x1050     59.95    59.88  
   1600x1024     60.17  
   1400x1050     59.98  
   1600x900      59.99    59.94    59.95    59.82  
   1280x1024     60.02  
   1440x900      59.89  
   1400x900      59.96    59.88  
   1280x960      60.00  
   1440x810      60.00    59.97  
   1368x768      59.88    59.85  
   1360x768      59.80    59.96  
   1280x800      59.99    59.97    59.81    59.91  
   1152x864      60.00  
   1280x720      60.00    59.99    59.86    59.74  
   1024x768      60.04    60.00  
   960x720       60.00  
   928x696       60.05  
   896x672       60.01  
   1024x576      59.95    59.96    59.90    59.82  
   960x600       59.93    60.00  
   960x540       59.96    59.99    59.63    59.82  
   800x600       60.00    60.32    56.25  
   840x525       60.01    59.88  
   864x486       59.92    59.57  
   800x512       60.17  
   700x525       59.98  
   800x450       59.95    59.82  
   640x512       60.02  
   720x450       59.89  
   700x450       59.96    59.88  
   640x480       60.00    59.94  
   720x405       59.51    58.99  
   684x384       59.88    59.85  
   680x384       59.80    59.96  
   640x400       59.88    59.98  
   576x432       60.06  
   640x360       59.86    59.83    59.84    59.32  
   512x384       60.00  
   512x288       60.00    59.92  
   480x270       59.63    59.82  
   400x300       60.32    56.34  
   432x243       59.92    59.57  
   320x240       60.05  
   360x202       59.51    59.13  
   320x180       59.84    59.32  
DP-1 disconnected (normal left inverted right x axis y axis)
HDMI-1 disconnected (normal left inverted right x axis y axis)
DP-2 connected 4384x2466+0+0 (normal left inverted right x axis y axis) 525mm x 295mm
   3840x2160     60.00*+  50.00    30.00  
   3360x1890     60.00  
   2560x1440     60.00  
   1920x1080     60.00  
   640x480       59.94  
DP-3 connected 4384x2466+4384+0 (normal left inverted right x axis y axis) 525mm x 295mm
   3840x2160     60.00*+  50.00    30.00  
   3360x1890     60.00  
   2560x1440     60.00  
   1920x1080     60.00  
   640x480       59.94  
DP-4 disconnected (normal left inverted right x axis y axis)
DP-5 disconnected (normal left inverted right x axis y axis)

```

## Logitech mouse fix

```
shams@shams-ubuntu:~$ sudo hciconfig hci0 sspmode 1
shams@shams-ubuntu:~$ sudo hciconfig hci0 down
shams@shams-ubuntu:~$ sudo hciconfig hci0 up
```