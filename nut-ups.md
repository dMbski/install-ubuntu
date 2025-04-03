# Install nut from apt
Verified on Ubuntu Linux 24.04.2
Webmin version 	2.303

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
When nut-scanner is not working, showing broken dependecies look which and make ln to broken files
ex
```
sudo ln -s /usr/lib/x86_64-linux-gnu/libusb-1.0.so.0.4.0 /usr/lib/x86_64-linux-gnu/libusb.so
sudo ln -s /usr/lib/x86_64-linux-gnu/libupsclient.so.6.0.1 /usr/lib/x86_64-linux-gnu/libupsclient.so
sudo ln -s /usr/lib/x86_64-linux-gnu/libavahi-client.so.3.2.9 /usr/lib/x86_64-linux-gnu/libavahi-client.so
```
## Checking owner and mode
```
Name      Owner      Mode
/etc/nut  root:root  0755
/etc/nut/*  root:nut  0640
```
# Configuration
## nut.conf
```
#  nut.conf
#  MODE=netserver
MODE=standalone
```
## ups.conf
if one have only one ups hash lines like bellow (too speciffic file could  be problematic, not only when change ups)
```
#  ups.conf
maxretry = 3
[nutapc]
        driver = "usbhid-ups"
        port = "auto"
        vendorid = "051D"
#        productid = "0002"
        desc = "Back-UPS ES 700G"
#        vendor = "APC"
        ignorelb
        default.ups.delay.shutdown = 60
        override.battery.charge.low = 30
        override.battery.charge.warning = 40
        override.battery.runtime.low = 300

```
## upsd.conf
```
#        upsd.conf
LISTEN 127.0.0.1 3493
```
## upsd.users
```
#        upsd.users
[admin]
  password = pass
  instcmds = ALL
  actions = SET FSD
  upsmon primary
```
## upsmon.conf
```
#        upsmon.conf
RUN_AS_USER root
MONITOR nutapc@localhost 1 admin pass primary
MINSUPPLIES 1
SHUTDOWNCMD "/sbin/shutdown -h +0"
NOTIFYCMD /usr/sbin/upssched
POLLFREQ 15
POLLFREQALERT 1
HOSTSYNC 15
DEADTIME 15
POWERDOWNFLAG "/etc/killpower"
# NOTIFYMSG - change messages sent by upsmon when certain events occur
NOTIFYMSG ONLINE	"UPS %s on line power"
NOTIFYMSG ONBATT	"UPS %s on battery"
NOTIFYMSG LOWBATT	"UPS %s battery is low"
NOTIFYMSG FSD		"UPS %s: forced shutdown in progress"
NOTIFYMSG COMMOK	"Communications with UPS %s established"
NOTIFYMSG COMMBAD	"Communications with UPS %s lost"
NOTIFYMSG SHUTDOWN	"Auto logout and shutdown proceeding"
NOTIFYMSG REPLBATT	"UPS %s battery needs to be replaced"
NOTIFYMSG NOCOMM	"UPS %s is unavailable"
NOTIFYMSG NOPARENT	"upsmon parent process died - shutdown impossible"
NOTIFYMSG CAL		"UPS %s: calibration in progress"
NOTIFYMSG OFF		"UPS %s: administratively OFF or asleep"
NOTIFYMSG BYPASS	"UPS %s: on bypass (powered, not protecting)"
NOTIFYMSG NOTBYPASS	"UPS %s: no longer on bypass"
# NOTIFYFLAG - change behavior of upsmon when NOTIFY events occur
# NOTIFYFLAG ONLINE	SYSLOG+WALL
NOTIFYFLAG ONBATT	SYSLOG+WALL
NOTIFYFLAG LOWBATT	SYSLOG+WALL
NOTIFYFLAG FSD	SYSLOG+WALL
NOTIFYFLAG COMMOK	SYSLOG+WALL
NOTIFYFLAG COMMBAD	SYSLOG+WALL
NOTIFYFLAG SHUTDOWN	SYSLOG+WALL
# NOTIFYFLAG REPLBATT	SYSLOG+WALL
NOTIFYFLAG NOCOMM	SYSLOG+WALL
NOTIFYFLAG NOPARENT	SYSLOG+WALL
#
OFFDURATION 30
RBWARNTIME 86400
NOCOMMWARNTIME 300
FINALDELAY 5
```
## upssched.conf
```
#        upssched.conf
CMDSCRIPT /bin/upssched-cmd
AT ONBATT * START-TIMER onbattwarn 30
AT ONLINE * CANCEL-TIMER onbattwarn
AT ONLINE * EXECUTE ups-back-on-line
AT ONBATT * EXECUTE onbatt
AT ONLINE * EXECUTE onpower

```
## upssched-cmd in /usr/bin/
root:root 0755

```
case $1 in
    onbatt)
    	logger -t upssched-cmd "$message"
    	;;
	onbattwarn)
		# Send a notification mail
		echo "The UPS has been on battery for awhile"
		;;
	ups-back-on-line)
		# Some generic message
        echo "The UPS returns on line"
		;;
	upsgone)
		logger -t upssched-cmd "The communication with UPS has been gone for awhile"
		;;
	*)
		logger -t upssched-cmd "Unrecognized command: $1"
		;;
esac
```

# Install nut Module for Webmin
Repository at:
https://sourceforge.net/projects/nut-monitor-webmin-module/files/0.2.2/
## configure like bellow
![Clipboard_03-23-2025_01](https://github.com/user-attachments/assets/01ebee3c-92e6-4607-9d63-67cd994ab10a)

# i to wszystko



