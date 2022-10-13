# Wireshark and WiFi

* https://wiki.wireshark.org/Wi-Fi
* https://wiki.wireshark.org/CaptureSetup/WLAN
* https://wiki.wireshark.org/HowToDecrypt802.11


* Wireshark has a "Wireless Toolbar" not shown by default. Toggle it in "View" settings.

## Scanning different channels

*Set channel:*

```
sudo iw dev wlan0 set channel X
```

*Go through channel once:*

```
for c in {1..14}; do sudo iw dev wlan0 set channel $c && sleep 1; done
```

*Display filter in wireshark for frequency:*

```
wlan_radio.frequency == XXXX
```

## Sorting out

A complement to the standard Statistics -> Conversations(fiter for IEEE 802.11) is WLAN-Traffic under the Wireless menu. This will get you a good overview of what have been captured with different nets, base stations and traffic.

### 802.11 Types and subtypes

* [802.11 Frame Types, Wikipedia](https://en.wikipedia.org/wiki/802.11_Frame_Types)

**Type 0: Management**

Negotiates and control the relationships between AP and clients.

Notable subtypes:

* (8)Beacon - Basic network information and timestamps, sent out about 10 times/second.
* (4-5)Probe - Sent out by clients to find APs.
* (11)Authentication - What it sounds like, same frame used by both client and AP.
* (0-1)Association - After authentication, request and response.
* (12)Deauthentication - By client or AP, "leaves the network".

**Type 1: Control**

Aids in the delivery of data frames.

Notable subtypes:

* (11)RTS - Request-to-send
* (12)CTS - Clear-to-send
* (13)ACK - Sent after receiving a unicast packet.

**Type 2: Data**

The actual data/information.

Notable subtypes:

* (0) Data
* (4) Null Function - Example: Before and after going into power saving mode.s
* (8) QoS Data - Data with a QoS-field.
* (12) QoS Null - Equivalent of 4 but with QoS-field.

(**Type 3: Extension**)

### Filtering types

```
# Capture filter
type mgt | ctl | data

# Display filter
wlan.fc.type == 0 | 1 | 2 | 3
```

### Filtering subtypes

```
# Capture filter
subtype beacon | probe-req | probe-resp

# Display filter
wlan.fc.subtype == 0 | 1 | ..
```

## Additional filtering

### Capture filters

* [Tcpdump pcap-filter manpange](https://www.tcpdump.org/manpages/pcap-filter.7.html)

*Common/userful:*

```
wlan addr1 XX:XX:XX:XX:XX:XX
```

### Display filters

*Main categories:*

* ieee80211.
* wlan.

```
# Source and destination addresses
wlan.sa | da

# Transmitter and receiver addresses
wlan.ta | ra

# WPA authentication packets:
eapol
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