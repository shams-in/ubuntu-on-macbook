https://wiki.t2linux.org/

https://github.com/marcosfad/mbp-ubuntu
https://github.com/t2linux/wiki/blob/a4b46a7cfbe7efcbb6a0b6111e22172b0f5c4a77/docs/guides/wifi.md

```
$ ioreg -l | grep RequestedFiles
C-4364s-B2/kauai.trx (ekans.trx)
C-4364s-B2/kauai-X3.clmb (kauai.clmb)
C-4364s-B2/P-kauai-X3_M-HRPN_V-u__m-7.5.txt (P-kauai_M-HRPN_V-u__m-7.5.txt)

cd /usr/share/firmware/wifi
right click > select "Show Original" to get the actual files
```

https://wiki.t2linux.org/guides/wifi/

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
