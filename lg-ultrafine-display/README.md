```
# check if iommu is enabled 
# if it doesn't return anything then follow the below step to enable it
$ sudo dmesg | grep -i "iommu"

# enable iommu (grub)
pass intel_iommu=on to kernal params

# authorize thunderbolt
# connect the display(s) and execute below command
$ boltctl list
$ boltctl authorize <uuid>

# verify
$ sudo xrandr -q

# [NOTE] FIX for rEFInd

{booted from rEFInd}
$ cat /proc/cmdline
root=UUID=e6f0ac73-30bb-4326-9747-d2d15eae0c77 ro efi=noruntime intel_iommu=on iommu=pt pcie-ports=compact nomodeset quiet splash vt.handoff=7 initrd=initrd.img-5.17.1-t2

{booted from Grub}
$ cat /proc/cmdline
BOOT_IMAGE=/vmlinuz-5.17.1-t2 root=UUID=e6f0ac73-30bb-4326-9747-d2d15eae0c77 ro efi=noruntime pcie_ports=compat quiet splash vt.handoff=7

{comparision}
ro efi=noruntime pcie-ports=compact quiet splash vt.handoff=7 intel_iommu=on iommu=pt nomodeset 
ro efi=noruntime pcie_ports=compat quiet splash vt.handoff=7

{worked}
removing --> nomodeset 
```
# Troubleshooting

```
$ dmesg -w

--- lg monitor inbuild usb hub logs ---

    [ 2741.777678] usb 3-2: new high-speed USB device number 3 using xhci_hcd
    [ 2741.905752] usb 3-2: device descriptor read/64, error -71
    [ 2742.165698] usb 3-2: New USB device found, idVendor=043e, idProduct=9a61, bcdDevice=52.49
    [ 2742.165711] usb 3-2: New USB device strings: Mfr=1, Product=2, SerialNumber=0
    [ 2742.165715] usb 3-2: Product: USB2.1 Hub
    [ 2742.165718] usb 3-2: Manufacturer: LG Electronics Inc.
    [ 2742.167086] hub 3-2:1.0: USB hub found
    [ 2742.171215] hub 3-2:1.0: 4 ports detected
    [ 2742.299479] usb 3-2: USB disconnect, device number 3

--- lg monitor thunderbolt logs ---

    [  +0.017675] thunderbolt 0-3: new device found, vendor=0x1e device=0x1115
    [  +0.000007] thunderbolt 0-3: LG Electronics UltraFine 4K
    [  +0.139719] thunderbolt 0-103: new device found, vendor=0x1e device=0x1115
    [  +0.000004] thunderbolt 0-103: LG Electronics UltraFine 4K


    [    1.201770] thunderbolt 0000:00:0d.3: can't derive routing for PCI INT A
    [    1.201772] thunderbolt 0000:00:0d.3: PCI INT A: not connected



lsmod | grep thund  
thunderbolt           315392  0

--- debug ---

echo -n "0000:04:00.0" > /sys/bus/pci/drivers/xhci_hcd/unbind
echo -n "0000:04:00.0" > /sys/bus/pci/drivers/xhci_hcd/bind

[   58.215005] xhci_hcd 0000:03:00.0: xHCI host controller not responding, assume dead
[   58.215031] xhci_hcd 0000:03:00.0: HC died; cleaning up
```