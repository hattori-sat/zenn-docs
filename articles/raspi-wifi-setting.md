---
title: "静的IPアドレスを設定したラズパイのSDカードを異なるラズパイに差した時に自動でWi-fiにつながるようにする方法"
emoji: "📑"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Debian", "RaspberryPi", "NetworkManager"]
published: true
---

# 1. 概要
長くなってしまったので1章で解決策を書いて、2章以降で解析した結果を載せるという構成で記事を書いています。

## 1.1. 前提条件
- raspberrypi4
- raspberrypiOS(bookworm)
- SDカードに自分で最新ソフトを焼き込み
- wifiの設定はSDカードに焼き込む時に設定

## 1.2. 悩み
2台の同種のraspberrypiがあったとき、あるraspberrypiでは問題なくWi-fiに繋がっていた。そのraspberrypiでは静的ipアドレスを設定していた。
そのSDカードをもう一台のraspberrypiに差すと起動はするがWi-fiに繋がらない。

## 1.3. 解決策
根本的な解決にはなっていないかもしれませんが、`/etc/NetworkManager/system-connections/preconfigured.nmconnection `のuuidをラズパイ起動時にランダムに変更することでWi-fiに繋がるようになった。  

なお、`preconfigured`となっているのは、私がSDカードに焼き込む際にWi-fiの設定をしているからであり、存在する`*.nmconnection`を変更してもらえば同じように動作すると思われる。

```bash
pi@raspi:~ $ sudo cat /etc/NetworkManager/system-connections/preconfigured.nmconnection 
[connection]
id=preconfigured
uuid=xxx
type=wifi
[wifi]
mode=infrastructure
ssid=xxx
hidden=false
[ipv4]
method=manual
address1=192.168.0.180/24
[ipv6]
addr-gen-mode=default
method=auto
[proxy]
[wifi-security]
key-mgmt=wpa-psk
psk=xxx
```

起動時に動作させるshell scriptを作成します。

```bash
sudo vim /usr/local/bin/update_nm_uuid.sh
```

内容は以下のとおりです。UUIDを生成して、sedで置換しているだけです。

```bash
#!/bin/bash

# Path to the .nmconnection file
CONNECTION_FILE="/etc/NetworkManager/system-connections/preconfigured.nmconnection"

# Generate a new UUID. なぜかuuidgenが使えなかったのでworkaround
NEW_UUID=$(python3 -c "import uuid; print(uuid.uuid4())")

# Update the UUID in the .nmconnection file
sed -i "s/^uuid=.*/uuid=$NEW_UUID/" "$CONNECTION_FILE"

# Set correct permissions (NetworkManager requires these)
chmod 600 "$CONNECTION_FILE"

# Reload the NetworkManager connections
nmcli connection reload

```

先ほど作成したshell scriptに実行権限を付与します。

```bash
sudo chmod +x /usr/local/bin/update_nm_uuid.sh
```

次に起動時に先ほど作成したshell scriptを呼ぶサービスを作成します。(上記shell scriptを呼べればどんな手段でも良いと思います。serviceにした方が追いやすいかなと思い、このようにしています。)

```bash
sudo vim /etc/systemd/system/update-nm-uuid.service
```

ファイルの中身は以下のとおりです。

```ini
[Unit]
Description=Update NetworkManager UUID
After=network-manager.service

[Service]
Type=oneshot
ExecStart=/usr/local/bin/update_nm_uuid.sh

[Install]
WantedBy=multi-user.target

```

そして、以下のようにserviceを有効化してstartします。そして、再起動します。

```bash
sudo systemctl enable update-nm-uuid.service
sudo systemctl start update-nm-uuid.service
sudo reboot
```

これで私の環境では、1.2.節の問題を解決することができました。

以下、https://networkmanager.dev/docs/api/latest/NetworkManager.conf.html から引用。
>Note that when NetworkManager gets restarted, it stores the previous state in /run/NetworkManager; in particular it saves the UUID of the connection that was previously active so that it can be activated again after the restart. Therefore, keep-configuration does not have any effect on service restart.

ドキュメントを読み漁ったわけではないので憶測になりますが、UUIDが同じだと前回と同じ設定でネットに接続してしまうため失敗するものと思っています。前と同じ設定なのに、実際はraspberrypiのmacアドレスが異なるため繋がらなくなるのではないかと思います。

----

# 2. configファイルの設定を確認

bootパーティションの構成を確認。config.txtを見てみる。

```bash
pi@raspi:~ $ ls -al /boot
total 53504
drwxr-xr-x  3 root root     4096 Nov 19 22:43 .
drwxr-xr-x 18 root root     4096 Nov 19 22:44 ..
-rw-r--r--  1 root root       92 Nov 19 22:32 cmdline.txt
-rw-r--r--  1 root root   238336 Oct  9 03:49 config-6.6.51+rpt-rpi-2712
-rw-r--r--  1 root root   238325 Oct  9 03:49 config-6.6.51+rpt-rpi-v8
-rw-r--r--  1 root root       91 Nov 19 22:32 config.txt
drwxr-xr-x  4 root root     4096 Jan  1  1970 firmware
-rw-r--r--  1 root root 17847637 Nov 19 22:43 initrd.img-6.6.51+rpt-rpi-2712
-rw-r--r--  1 root root 17843486 Nov 19 22:43 initrd.img-6.6.51+rpt-rpi-v8
lrwxrwxrwx  1 root root       18 Nov 19 22:43 issue.txt -> firmware/issue.txt
lrwxrwxrwx  1 root root       17 Nov 19 22:32 overlays -> firmware/overlays
-rw-r--r--  1 root root       83 Oct  9 03:49 System.map-6.6.51+rpt-rpi-2712
-rw-r--r--  1 root root       83 Oct  9 03:49 System.map-6.6.51+rpt-rpi-v8
-rw-r--r--  1 root root  9292371 Oct  9 03:49 vmlinuz-6.6.51+rpt-rpi-2712
-rw-r--r--  1 root root  9284374 Oct  9 03:49 vmlinuz-6.6.51+rpt-rpi-v8v
```

config.txtの中身を確認。`/boot/firmware/config.txt`に移動したらしい。

```bash
pi@raspi:~ $ cat /boot/config.txt 
DO NOT EDIT THIS FILE

The file you are looking for has moved to /boot/firmware/config.txt
```

移動した先のconfig.txtを確認。

```bash
pi@raspi:~ $ cat /boot/firmware/config.txt 
# For more options and information see
# http://rptl.io/configtxt
# Some settings may impact device functionality. See link above for details

# Uncomment some or all of these to enable the optional hardware interfaces
#dtparam=i2c_arm=on
#dtparam=i2s=on
#dtparam=spi=on

# Enable audio (loads snd_bcm2835)
dtparam=audio=on

# Additional overlays and parameters are documented
# /boot/firmware/overlays/README

# Automatically load overlays for detected cameras
camera_auto_detect=1

# Automatically load overlays for detected DSI displays
display_auto_detect=1

# Automatically load initramfs files, if found
auto_initramfs=1

# Enable DRM VC4 V3D driver
dtoverlay=vc4-kms-v3d
max_framebuffers=2

# Don't have the firmware create an initial video= setting in cmdline.txt.
# Use the kernel's default instead.
disable_fw_kms_setup=1

# Run in 64-bit mode
arm_64bit=1

# Disable compensation for displays with overscan
disable_overscan=1

# Run as fast as firmware / board allows
arm_boost=1

[cm4]
# Enable host mode on the 2711 built-in XHCI USB controller.
# This line should be removed if the legacy DWC2 controller is required
# (e.g. for USB device mode) or if USB support is not required.
otg_mode=1

[cm5]
dtoverlay=dwc2,dr_mode=host

[all]
```

ここにはネットワーク関係の事柄は記載されていなさそう。

----

# 3. networkの設定が書かれているファイルを確認
ネットで調べると`/etc/wpa_supplicant/wpa_supplicant.conf`に設定ファイルがあると書かれている。出てくる場所には設定ファイルはなかった。NetworkManagerはwpa_supplicantを使用しなくなる可能性があるらしい[1]のでたぶんここにはないのだろう。

```bash
pi@raspi:~ $ ls -al /etc/wpa_supplicant/
total 56
drwxr-xr-x   2 root root  4096 Nov 19 22:33 .
drwxr-xr-x 132 root root 12288 Nov 19 22:45 ..
-rwxr-xr-x   1 root root   937 Aug  6 04:07 action_wpa.sh
-rw-r--r--   1 root root 25569 Aug  6 04:07 functions.sh
-rwxr-xr-x   1 root root  4696 Aug  6 04:07 ifupdown.sh
```

`/etc/`以下を探してみると以下が該当しそう。

```bash
pi@raspi:~ $ ls -al /etc/NetworkManager/
total 40
drwxr-xr-x   7 root root  4096 Nov 19 22:33 .
drwxr-xr-x 132 root root 12288 Nov 19 22:45 ..
drwxr-xr-x   2 root root  4096 Jul 24  2023 conf.d
drwxr-xr-x   5 root root  4096 Nov 19 22:33 dispatcher.d
drwxr-xr-x   2 root root  4096 Jul 24  2023 dnsmasq.d
drwxr-xr-x   2 root root  4096 Jul 24  2023 dnsmasq-shared.d
-rw-r--r--   1 root root    98 Jul 24  2023 NetworkManager.conf
drwxr-xr-x   2 root root  4096 Nov 19 22:44 system-connections
```

該当しそうなconfファイルを見てみる。ただの設定という感じ。

```bash
pi@raspi:~ $ cat /etc/NetworkManager/NetworkManager.conf 
[main]
plugins=ifupdown,keyfile

[ifupdown]
managed=false

[device]
wifi.scan-rand-mac-address=no
```

`preconfigured.nmconnection`が設定ファイルらしい。以下の *xxx* は伏せ字にしています。

```bash
pi@raspi:~ $ sudo cat /etc/NetworkManager/system-connections/preconfigured.nmconnection 
[connection]
id=preconfigured
uuid=xxx
type=wifi
[wifi]
mode=infrastructure
ssid=xxx
hidden=false
[ipv4]
method=auto
[ipv6]
addr-gen-mode=default
method=auto
[proxy]
[wifi-security]
key-mgmt=wpa-psk
psk=xxx
```

----

現在のraspberrypiのIPアドレスを確認します。(抜粋)

```bash
pi@raspi:~ $ ifconfig

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1440
        inet 192.168.0.178  netmask 255.255.255.0  broadcast 192.168.0.255
```

今回の条件では静的IPアドレスを使用しているため、以下のように`*.nmconnection `を変更(*は自分の環境にあるものを入れてください。)

```bash
pi@raspi:~ $ sudo cat /etc/NetworkManager/system-connections/preconfigured.nmconnection 
[connection]
id=preconfigured
uuid=xxx
type=wifi
[wifi]
mode=infrastructure
ssid=xxx
hidden=false
[ipv4]
method=manual
address1=192.168.0.180/24
[ipv6]
addr-gen-mode=default
method=auto
[proxy]
[wifi-security]
key-mgmt=wpa-psk
psk=xxx
```

設定した静的IPになっていることを確認。(該当部抜粋)

```bash
pi@raspi:~ $ ip addr show
3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.0.180/24 brd 192.168.0.255 scope global noprefixroute wlan0
```

daemonの動きについては以下を参照しました。`nm_config_setup`←このあたりとか
- https://gitlab.freedesktop.org/NetworkManager/NetworkManager/-/blob/main/src/core/main.c?ref_type=heads
- https://gitlab.freedesktop.org/NetworkManager/NetworkManager/-/blob/main/src/core/nm-config.c?ref_type=heads
- https://gitlab.freedesktop.org/NetworkManager/NetworkManager/-/blob/main/src/core/nm-config.h?ref_type=heads


----

# 4. 参考文献
1. Raspberry Pi, Forma, https://forums.raspberrypi.com/viewtopic.php?t=357623