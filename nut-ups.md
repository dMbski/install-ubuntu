# Install nut from apt
## Checking ups connection and nut 
### lsusb
List USB devices and 
```
...
Bus 002 Device 003: ID 051d:0002 American Power Conversion Uninterruptible Power Supply
...
```

### sudo nut-scanner
List nut supported devices-ups
```
...
[nutdev1]
        driver = "usbhid-ups"
        port = "auto"
        vendorid = "051D"
        productid = "0002"
        product = "Back-UPS ES 700G FW:871.O2 .I USB FW:O2"
        serial = "3B88910X16463"
        vendor = "APC"
        bus = "002"
        device = "003"
        busport = "005"
...
```

## Checking owner and mode
Name      Owner      Mode
/etc/nut  root:root  0755
/etc/nut/*  root:nut  0640

# Configuration
## nut.conf
```
#  nut.conf
#  MODE=netserver
MODE=standalone
```
## ups.conf
```
#  ups.conf
maxretry = 3
[nutapc]
        driver = "usbhid-ups"
        port = "auto"
        vendorid = "051D"
        productid = "0002"
        desc = "Back-UPS ES 700G"
        vendor = "APC"
        ignorelb
        default.ups.delay.shutdown = 60
        override.battery.charge.low = 30
        override.battery.charge.warning = 40
        override.battery.runtime.low = 300

```

