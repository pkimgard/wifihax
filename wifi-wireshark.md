# Wireshark and WiFi

* https://wiki.wireshark.org/Wi-Fi
* https://wiki.wireshark.org/CaptureSetup/WLAN
* https://wiki.wireshark.org/HowToDecrypt802.11


* Wireshark has a "Wireless Toolbar" not shown by default. Toggle it in "View" settings.

## Scanning different channels

*Set channel:*

```
sudo iw dev wlan0 channel X
```

*Go through channel once:*

```
for c in {1..14}; do sudo iw dev wlan0 set channel $c && sleep 1; done
```

*Display filter in wireshark for frequency:*

```
wlan_radio.frequency == XXXX
```

## Display filters

*Main categories:*

* ieee80211.
* wlan.

## Capture filter

* [Tcpdump pcap-filter manpange](https://www.tcpdump.org/manpages/pcap-filter.7.html)

*Common/userful:*

```
subtype beacon | probe-req | probe-resp
type ctl
wlan addr1 XX:XX:XX:XX:XX:XX
```

## Remote capturing

*Capture raw PCAP:*

```
sudo tcpdump -i wlan0 -w - -U
sudo dumpcap -i wlan0 -w - -P
sudo tshark -i wlan0 -w -
```

*Read raw PCAP from STDIN:*

```
sudo tcpdump -i wlan0 -w - -U | wireshark -k -i -
wireshark -k -i ./named-pipe
```

*Remote SSH capture:*

```
ssh adm@host "sudo -S tcpdump -i wlan0 -w - -U" | wireshark -k -i -
```

## Decryption

1. 802.11 Preferences
2. Protocols -> IEEE 802.11
3. Decryption Keys
4. Add keys before capture

For wpa-psk, generate with:

```
wpa_passphrase ESSID
```

## Coloring

* [Wlan coloring(Wireshark wiki)](https://wiki.wireshark.org/uploads/__moin_import__/attachments/ColoringRules/Wireshark-Wlan-ColouringRules.txt)