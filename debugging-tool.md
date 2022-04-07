wireshark

i can use wireshark to capture the usb traffic to and from the touchbar if that's what you mean
the configuration names were from lsusb but I think you can also read them from sysfs

basically, u need to wireshark capture the bce/usb bus on macos 

regarding issues with dumping traffic between the T2, i've had success with wireshark's companion terminal utility tshark