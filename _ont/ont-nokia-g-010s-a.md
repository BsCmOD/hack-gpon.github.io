---
title: Nokia G-010S-A
has_children: false
layout: default
parent: Nokia
---

# Hardware Specifications

|                  |                                          |
| ---------------- | ---------------------------------------- |
| Vendor/Brand     | Nokia                                    |
| Model            | G-010S-A                                 |
| ODM              | SourcePhotonics                          |
| ODM Product Code |                                          |
| Chipset          | Lantiq PEB98035                          |
| Flash            | 16 MB                                    |
| RAM              | 64 MB                                    |
| CPU              | MIPS 34Kc interAptiv                     |
| CPU Clock        | 400MHz                                   |
| System           | OpenWRT                                  |
| HSGMII           | Yes                                      |
| Optics           | SC/UPC                                   |
| IP address       | 192.168.1.10                             |
| Web Gui          | ✅ user `adminadmin`, password `ALC#FGU` |
| SSH              | ✅ user `ONTUSER`, password `SUGAR2A041` |
| Telnet           |                                          |
| Serial           | ✅ on SFP                                |
| Serial baud      | 115200                                   |
| Serial encoding  | 8-N-1                                    |
| Form Factor      | miniONT SFP                              |

{% include image.html file="g-010s-a.png"  alt="G-010S-A" caption="G-010S-A" %}
{% include image.html file="g-010s-a-teardown.jpg"  alt="G-010S-A Teardown" caption="G-010S-A Teardown" %}


## Modifying firmware

The Nokia G-010S-A can be flashed with the [Nokia G-010S-P](/ont-nokia-g-010s-p) firmware, provided the MTD layout has been changed beforehand to match the new one. For the full procedure, see the post on [lafibre.info](https://lafibre.info/remplacer-livebox/guide-de-connexion-fibre-directement-sur-un-routeur-voire-meme-en-2gbps/msg870551/#msg870551)

## List of software versions

- [https://github.com/hwti/G-010S-A/tree/main/firmwares](https://github.com/hwti/G-010S-A/tree/main/firmwares)

## Serial

The stick has a TTL 3.3v UART console (configured as 115200 8-N-1) that can be accessed from the SFP connector.

| USB TTL(UART) Adapter | SFP 20pins Molex connector |
| --------------------- | -------------------------- |
| 3.3V                  | pin #15 and #16            |
| TX                    | pin #3                     |
| RX                    | pin #6                     |
| GND                   | pin #14 and #10            |

{% include alert.html content="Try PIN 10 or other GND PINs if the connection doesn't work by using PIN 14." alert="Note"  icon="svg-warning" color="yellow" %}

{% include alert.html content="Some USB TTL adapters label TX and RX pins the other way around: try to swap them if the connection doesn't work." alert="Note"  icon="svg-warning" color="yellow" %}

# General Settings and Useful Commands

##  Disabling Dying Gasp
```sh
uci set gpon.gtc.nDyingGaspEnable='0'; uci commit gpon
```

## PLOAM

- PLOAM must be configured in HTTP manage page in section called SLID

## Set operator ID

This setting must be inserted in order to performs the others parameters modification
```sh
ritool set OperatorID 0000
```

## Set serial number (I.E. ALCLf123456d)

```sh
ritool set MfrID ALCL
ritool set G984Serial f123456d
ritool set YPSerialNum f123456d
```

## Set MAC address (I.E. 123456789ABC)

```sh
ritool set MACAddress 12:34:56:78:9A:BC
```

## Set Hardwar version (I.E. 3FE47211EGAA01)

```sh
ritool set HardwareVersion 3FE47211EGAA
ritool set ICS 01
```
## Set software version

Software verion must be changed by directly modifing firmware by using patches file which can be found in github page of [Nokia G-010-A](https://github.com/hwti/G-010S-A/tree/main/patches/0000_dropbear)
using specific script

## Set equipment ID (I.E. __________G-010G-Q__)

```sh
ritool set CleiCode __________
ritool set Mnemonic G-010G-Q__
```

## Upgrade firmware commands

First of all preparare an emergency TTL access

```sh
fw_setenv bootdelay 5
fw_setenv asc0 0
fw_setenv preboot
```

Verify which is the actual active partition with

```sh
upgradestatus
```

The update must be done with [PSCP](https://the.earth.li/~sgtatham/putty/latest/w64/pscp.exe) by using SCP protocol

```sh
pscp.exe -scp C:\3FE46398BGCB22.bin ONTUSER@192.168.1.10:/tmp
```

Then if active partition is 0 then use 1 if viceversa use the 0

```sh
mtd write /tmp/3FE46398BGCB22.bin image1
update_env_flag 1 
reboot
```

## Miscellaneous commands

```sh
cat /configs/image_version
cat /usr/etc/buildinfo
ritool dump
omciMgr
ifconfig eth0:1 192.168.1.10 netmask 255.255.255.0
```

# HW Modding

- [Nokia G-010S-A Pin 6 Iusse - Rsaxvc.net](https://rsaxvc.net/blog/2020/8/15/Nokia_G-010S-A_Pin_6_Issue.html)

# Miscellaneous Links

- [G-010S-A](https://github.com/hwti/G-010S-A)
- [Usage GPON module SFP in Spain](https://forum.mikrotik.com/viewtopic.php?t=116364&start=300)
- [Bypassing the HH3K up to 2.5Gbps using a BCM57810S NIC](https://www.dslreports.com/forum/r32230041-Internet-Bypassing-the-HH3K-up-to-2-5Gbps-using-a-BCM57810S-NIC)

