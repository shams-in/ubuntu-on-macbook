```
# linux kernel

shams@ub:~$ uname -a
Linux ub 5.17.1-t2 #1 SMP PREEMPT Tue Mar 29 11:02:19 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux

$ sudo apt install ./ios-firmware.deb

$ sudo nano /etc/NetworkManager/NetworkManager.conf

    [main]
    plugins=ifupdown,keyfile
    [ifupdown]
    managed=false
    [device]
    wifi.scan-rand-mac-address=no

$ sudo nano /etc/NetworkManager/conf.d/wifi_backend.conf

    #[device]
    #wifi.backend=iwd

$ sudo systemctl restart NetworkManager

$ reboot
```