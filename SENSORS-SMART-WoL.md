# Sensors for temperatures read

## install and configure packet
> sudo apt install lm-sensors

Start config creator
> sudo sensors-detect

Commit writting to config file at the end.

## show dettected sensors
>sudo sensors

# SMART applications 

## install
> sudo apt install smartmontools

### Check SMART capabilities
List block devices
> lsblk

Check SMART for sda
> sudo smartctl -i /dev/sda

Enable SMART if not enabled for device
> sudo smartctl -s on /dev/sda 

# Enable WoL - ethtool

## install ethtool
> sudo apt install ethtool

## Check ethernet card settings

List networks
>ifconfig

Show settings for enp1s0f0 card
>ethtool enp1s0f0

WoL capabilities are under:
```
Supports Wake-on: pumbg
Wake-on: g
```

### Create permament WoL
Create Cron Job starting at boot with command
> /usr/sbin/ethtool -s enp1s0f0 wol g

Check WOL type _g_ in capabillities above.

