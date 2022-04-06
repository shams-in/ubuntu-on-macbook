```
The Logitech M720 is labeled as Bluetooth Smart which is a.k.a Bluetooth Low Energy, BLE, or Bluetooth 4.0 (or newer).

'$ hcitool scan' will only show classic bluetooth.

The proper command is '$ hcitool lescan'.
$ hcitool lescan

To check version of Bluetooth supported in Ubuntu, use the command '$ hciconfig -a'.
$ hciconfig -a

Only one hci0 device should be present.

The HCI Version will need to be 4.0 or newer.

## connect to bluetooth from command line

# check the computer bluetooth
$ hcitool dev # note down device id (eg: hci0)

# scan for devices
$ hcitool -i hci0 scan

# pair and connect
$ bluetoothctl
[bluetooth]# trust FC:XX:XX:XX:XX:FE
[bluetooth]# connect FC:XX:XX:XX:XX:FE
[E7]# paired-devices
scan on

[NEW] Device F2:5B:D2:5A:88:21 M720 Triathlon (pair 1)
[NEW] Device F2:5B:D2:5A:88:24 M720 Triathlon (pair 2)

```

# enable Simple Pairing mode
$ sudo hciconfig hci0 sspmode 1
$ sudo hciconfig hci0 down
$ sudo hciconfig hci0 up

https://github.com/fin-ger/logitech-m720-config