\tableofcontents

# Create, activate or check monitor mode

## Change adapter mode

```
# Shutdown interface first
sudo ip link set wlan0 down

# Using iwconfig
sudo iwconfig wlan0 mode monitor
# Using iw
iw dev wlan0 set monitor none

# Bring up interface
sudo ip link set wlan0 up
```

## New sub-interface

```
sudo iw dev wlan0 interface add wlmon monitor
sudo ip link set wlmon up
```

*Delete:*

```
sudo iw dev wlmon del
```

## Check mode

```
iw dev wlan0 info
iw dev wlan0 info | grep type | cut -d " " -f 2
```

## Get interface information

```
sudo airmon-ng
iw list
iw dev wlan info
iw phy0 channels
lsusb
```

## Which channel am I currently using

```
iwlist wlp62s0 channel | grep Current | cut -d ":" -f 2
```

# Gett a USB WLAN adapter working in VM/Virtualbox

Host system needs:

* Virtualbox extension pack
* User that runs virtualbox to be in vboxuser

```
# Find adapter,
# Note if USB2 or 3,
# Note idVendor and idProduct (4 chars)
lsusb -v
```

In virtualbox VM settings:

* Settings -> USB
* Check correct USB version
* Add filter, empty for everything or insert idVendor/Product.

In VM:

* Install driver, obviously

```
sudo airmon-ng
lsusb

# Check if you have the correct hosts pressent
sudo vboxmanage list usbhost
```

# Adapter region setttings

```
iw reg get
iw reg set [SE | US | ..]
```

System defaults in: `/etc/default/cdra`.

# Power save mode

Sometimes the adapter goes down after some time due to power-saving options.

```
sudo iw dev wlan0 set power_save off | on
```

Also see if NetworkManager is overriding your settings.