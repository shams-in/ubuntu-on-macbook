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