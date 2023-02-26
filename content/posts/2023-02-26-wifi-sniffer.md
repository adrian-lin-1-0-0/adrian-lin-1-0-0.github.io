---
title: "Wifi Sniffer"
date: 2023-02-26T08:55:45+08:00
author: adrian
type: post
url: /2023/02/wifi-sniffer/
tags:
  - python
  - wifi
---


<style>
.lntable{
    max-height: 50em;
}
</style>

## 關於咖啡廳的 wifi router

咖啡廳wifi router的常見的攻擊 - `WiFi Sniffing`
> WiFi Sniffing:是指使用特殊的軟體工具來監聽傳輸在 Wi-Fi 網絡上的封包,包括帳號密碼這些敏感資訊.

要進行`WiFi Sniffing`,通常使用以下方法:

- 監聽 Wi-Fi 流量
- 假冒 Wi-Fi AP


我們將使用`scapy`<br>
[https://scapy.net/](https://scapy.net/)
> Scapy is a powerful interactive packet manipulation program. 

```python
pip3 install scapy
```

## 監聽 Wi-Fi 流量

使用`scapy`監聽`wlan0` interface上的流量,
如果是 tcp 就輸出摘要.

```python
from scapy.all import *

def packet_handler(pkt):
    if pkt.haslayer(TCP):
        print(pkt.summary())

sniff(iface="wlan0", prn=packet_handler)
```

## 假冒 Wi-Fi AP

使用`scapy.sendp`在無窮迴圈中broadcasts `Beacon frame`
> Beacon frame : Beacon frame is one of the management frames in IEEE 802.11 based WLANs. It contains all the information about the network. Beacon frames are transmitted periodically, they serve to announce the presence of a wireless LAN and to synchronise the members of the service set. - [wikipedia](https://en.wikipedia.org/wiki/Beacon_frame)


```python
from scapy.all import *

def packet_handler(pkt):
    if pkt.haslayer(TCP):
        print(pkt.summary())

def start_fake_ap():
    ap_name = "Free WiFi"
    ap_mac = "00:11:22:33:44:55"

    beacon = Dot11Beacon(cap="ESS+privacy")
    essid = Dot11Elt(ID="SSID",info=ap_name, len=len(ap_name))
    dsset = Dot11Elt(ID="DSset",info="\x01")
    rsn = Dot11Elt(ID='RSNinfo', info=(
            '\x01\x00'                 #RSN Version 1
            '\x00\x0f\xac\x02'         #Group Cipher Suite : 00-0f-ac TKIP
            '\x02\x00'                 #2 Pairwise Cipher Suites (next two lines)
            '\x00\x0f\xac\x04'         #AES Cipher
            '\x00\x0f\xac\x02'         #TKIP Cipher
            '\x01\x00'                 #1 Authentication Key Managment Suite (line below)
            '\x00\x0f\xac\x02'         #Pre-Shared Key
        ))
    frame = RadioTap()/beacon/essid/dsset/rsn

    while True:
        sendp(frame, iface="wlan0")

sniff(iface="wlan0", prn=packet_handler)
start_fake_ap()

```
