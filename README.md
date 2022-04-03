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