---
title: "RaspberryPiの環境設定"
free: true
---

# 1. 実行環境
環境設定の前に、私の環境を以下に示します。

- RaspberryPi4 RAM 8GB
- Camera Module 3 (IMX708)
- RaspberryPi OS 64bit recommended (WindowsでSDに書き込み)
- SD card 16GB
- (MacでVNC viewer)


# 2. 初期設定 (HDMIでディスプレイに映している場合は不要)
SD cardに書き込んだ後は、VNCが繋がらないと思うのでssh通信します。
名前等とパスワードは、SDカードに書き込む際に決めていると思います。

以下を実行する。
```bash
ssh hattori@raspi
```

結果、以下が表示される。こうなればraspberryPiは正常に動いている。

```bash
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sat Mar 16 00:13:33 2024
hattori@raspi:~ $ 
```

version等も以下に記載しておきます。

```bash
hattori@raspi:~ $ lsb_release -a

No LSB modules are available.
Distributor ID:	Debian
Description:	Debian GNU/Linux 12 (bookworm)
Release:	12
Codename:	bookworm


hattori@raspi:~ $ uname -a

Linux raspi 6.6.20+rpt-rpi-v8 #1 SMP PREEMPT Debian 1:6.6.20-1+rpt1 (2024-03-07) aarch64 GNU/Linux
```

早速、セットアップしていきます。  
まずは、sudo raspi-configでVNC通信を有効にします。

```bash
sudo raspi-config
```
をすると以下の画面が開くと思います。

![raspi-config](/images/raspi-camera/1.png)

3.INTERFACESのI2CとVNCをenableにしておきます。

ここまでできたら、一旦再起動します。

```bash
sudo reboot
```

:::message
以下のコマンドは打っても打たなくても大丈夫です。
```bash
sudo apt-get update
sudo apt-get upgrade -y
```
:::

# 3. cameraの設定
　[picamera2の公式のドキュメント](https://datasheets.raspberrypi.com/camera/picamera2-manual.pdf)を参考にします。

## 3.1. licameraが動いているかどうか確認

まずは、pdfにある通り以下のコマンドを打ってみました。  

```bash
hattori@raspi:~ $ rpicam-hello

[0:07:39.521417654] [1965]  INFO Camera camera_manager.cpp:284 libcamera v0.2.0+46-075b54d5
[0:07:39.562125251] [1968]  WARN RPiSdn sdn.cpp:39 Using legacy SDN tuning - please consider moving SDN inside rpi.denoise
[0:07:39.564421261] [1968]  INFO RPI vc4.cpp:447 Registered camera /base/soc/i2c0mux/i2c@1/imx708@1a to Unicam device /dev/media2 and ISP device /dev/media3
[0:07:39.564654747] [1968]  INFO RPI pipeline_base.cpp:1144 Using configuration file '/usr/share/libcamera/pipeline/rpi/vc4/rpi_apps.yaml'
Made X/EGL preview window
Mode selection for 2304:1296:12:P
    SRGGB10_CSI2P,1536x864/0 - Score: 3400
    SRGGB10_CSI2P,2304x1296/0 - Score: 1000
    SRGGB10_CSI2P,4608x2592/0 - Score: 1900
Stream configuration adjusted
[0:07:40.725946928] [1965]  INFO Camera camera.cpp:1183 configuring streams: (0) 2304x1296-YUV420 (1) 2304x1296-SBGGR10_CSI2P
[0:07:40.726534589] [1968]  INFO RPI vc4.cpp:611 Sensor: /base/soc/i2c0mux/i2c@1/imx708@1a - Selected sensor format: 2304x1296-SBGGR10_1X10 - Selected unicam format: 2304x1296-pBAA

```

:::alert
このとき、以下のようにならない場合は、ハード的に接触が悪い可能性があるのでラズパイの電源を切って差し直しましょう。結構奥まで差し込むことができます。  
なお、電源を切らずに行うとショートして壊れる可能性ありです。
:::

## 3.2. picamera2のインストール

```bash
hattori@raspi:~ $ sudo apt install -y python3-picamera2

Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages will be upgraded:
  python3-picamera2
1 upgraded, 0 newly installed, 0 to remove and 124 not upgraded.
Need to get 59.6 kB of archives.
After this operation, 3,072 B of additional disk space will be used.
Get:1 http://archive.raspberrypi.com/debian bookworm/main arm64 python3-picamera2 all 0.3.19-1 [59.6 kB]
Fetched 59.6 kB in 3s (21.5 kB/s)            
apt-listchanges: Reading changelogs...
(Reading database ... 127434 files and directories currently installed.)
Preparing to unpack .../python3-picamera2_0.3.19-1_all.deb ...
Unpacking python3-picamera2 (0.3.19-1) over (0.3.17-1) ...
Setting up python3-picamera2 (0.3.19-1) ...
```

## 3.3. (optional) sampleコード
もうすでにここまでで動くことは一応確認できたはずです。  
ここでは、picamera2が動いているかをsampleコードで確認します。

一応、githubにコードをあげているのでクローンして使うことにします。

```terminal
hattori@raspi:~ $ ls
Bookshelf  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos

hattori@raspi:~ $ cd Documents/

hattori@raspi:~/Documents $ ls

hattori@raspi:~/Documents $ mkdir camera

hattori@raspi:~/Documents $ cd camera/

hattori@raspi:~/Documents/camera $ git clone https://github.com/hattori-sat/raspi-camera.git
Cloning into 'raspi-camera'...
remote: Enumerating objects: 68, done.
remote: Counting objects: 100% (68/68), done.
remote: Compressing objects: 100% (53/53), done.
remote: Total 68 (delta 20), reused 51 (delta 11), pack-reused 0
Receiving objects: 100% (68/68), 18.30 KiB | 720.00 KiB/s, done.
Resolving deltas: 100% (20/20), done.

hattori@raspi:~/Documents/camera $ ls
raspi-camera

hattori@raspi:~/Documents/camera $ cd raspi-camera/

hattori@raspi:~/Documents/camera/raspi-camera $ ls
function_test  memo.md  README.md

hattori@raspi:~/Documents/camera/raspi-camera $ cd function_test/

hattori@raspi:~/Documents/camera/raspi-camera/function_test $ ls
cap_movie_to_image.py  capture_60hz.py  sample.py

hattori@raspi:~/Documents/camera/raspi-camera/function_test $ mkdir imgs

hattori@raspi:~/Documents/camera/raspi-camera/function_test $ python3 sample.py 
[0:31:03.094892817] [2258]  INFO Camera camera_manager.cpp:284 libcamera v0.2.0+46-075b54d5
[0:31:03.124465105] [2261]  WARN RPiSdn sdn.cpp:39 Using legacy SDN tuning - please consider moving SDN inside rpi.denoise
[0:31:03.127062321] [2261]  INFO RPI vc4.cpp:447 Registered camera /base/soc/i2c0mux/i2c@1/imx708@1a to Unicam device /dev/media2 and ISP device /dev/media3
[0:31:03.127503117] [2261]  INFO RPI pipeline_base.cpp:1144 Using configuration file '/usr/share/libcamera/pipeline/rpi/vc4/rpi_apps.yaml'
[0:31:03.130648017] [2258]  INFO Camera camera_manager.cpp:284 libcamera v0.2.0+46-075b54d5
[0:31:03.159131011] [2264]  WARN RPiSdn sdn.cpp:39 Using legacy SDN tuning - please consider moving SDN inside rpi.denoise
[0:31:03.161779505] [2264]  INFO RPI vc4.cpp:447 Registered camera /base/soc/i2c0mux/i2c@1/imx708@1a to Unicam device /dev/media2 and ISP device /dev/media3
[0:31:03.162058190] [2264]  INFO RPI pipeline_base.cpp:1144 Using configuration file '/usr/share/libcamera/pipeline/rpi/vc4/rpi_apps.yaml'
[0:31:03.171828447] [2258]  INFO Camera camera.cpp:1183 configuring streams: (0) 640x480-XBGR8888 (1) 1536x864-SBGGR10_CSI2P
[0:31:03.172650445] [2264]  INFO RPI vc4.cpp:611 Sensor: /base/soc/i2c0mux/i2c@1/imx708@1a - Selected sensor format: 1536x864-SBGGR10_1X10 - Selected unicam format: 1536x864-pBAA
QStandardPaths: wrong permissions on runtime directory /run/user/1000, 0770 instead of 0700ß
```
これで、imgsフォルダに画像が保存されているはずです。

# 4. OpenCVのインストール
昔は全然意味のわからないコマンドを打ち続けて失敗することを繰り返していましたが、今は簡単にインストールできるようです。

OpenCVは画像処理などで幅広く使われています。

##　update and upgrade
人や時間、ものなど環境によって変わると思いますが、以下にターミナルの結果を示しておきます。（一部抜粋）

```bash
hattori@raspi:~ $ sudo apt update
Hit:1 http://deb.debian.org/debian bookworm InRelease
Hit:2 http://deb.debian.org/debian-security bookworm-security InRelease
Hit:3 http://deb.debian.org/debian bookworm-updates InRelease
Hit:4 http://archive.raspberrypi.com/debian bookworm InRelease 
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
124 packages can be upgraded. Run 'apt list --upgradable' to see them.

hattori@raspi:~ $ sudo apt upgrade -y
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Calculating upgrade... Done
The following packages were automatically installed and are no longer required:
  libcamera0.2 libraspberrypi0 libwpe-1.0-1 libwpebackend-fdo-1.0-1
Use 'sudo apt autoremove' to remove them.
The following NEW packages will be installed:
  gtk-nop gui-updater libcamera0.3 libhwloc-plugins libhwloc15 libopencv-calib3d406 libopencv-core406 libopencv-dnn406 libopencv-features2d406
  libopencv-flann406 libopencv-imgproc406 libopencv-objdetect406 libprotobuf32 libtbb12 libtbbbind-2-5 libtbbmalloc2 libxnvctrl0
  linux-headers-6.6.31+rpt-common-rpi linux-headers-6.6.31+rpt-rpi-2712 linux-headers-6.6.31+rpt-rpi-v8 linux-image-6.6.31+rpt-rpi-2712
  linux-image-6.6.31+rpt-rpi-v8 linux-kbuild-6.6.31+rpt pastebinit python3-pycryptodome
The following packages will be upgraded:
  arandr bsdextrautils bsdutils chromium-browser chromium-browser-l10n chromium-codecs-ffmpeg-extra dpkg dpkg-dev eject fdisk firefox ghostscript
  gir1.2-gtk-3.0 gstreamer1.0-alsa gstreamer1.0-plugins-base gstreamer1.0-x gtk-update-icon-cache gui-pkinst less libarchive13 libblkid1 libc-bin
  libc-dev-bin libc-devtools libc-l10n libc6 libc6-dbg libc6-dev libcamera-ipa libcamera-tools libcamera0.2 libdav1d6 libdpkg-perl libfdisk1
  libfm-data libfm-extra4 libfm-gtk-data libfm-gtk4 libfm-modules libfm4 libglib2.0-0 libglib2.0-bin libglib2.0-data libgs-common libgs10
  libgs10-common libgstreamer-gl1.0-0 libgstreamer-plugins-base1.0-0 libgtk-3-0 libgtk-3-common libjavascriptcoregtk-4.1-0 libmount1 libndp0
  libneatvnc0 libpipewire-0.3-0 libpipewire-0.3-common libpipewire-0.3-modules libpisp-common libpisp1 libsmartcols1 libspa-0.2-bluetooth
  libspa-0.2-modules libuuid1 libvlc-bin libvlc5 libvlccore9 libwebkit2gtk-4.1-0 libwf-utils0 linux-headers-rpi-2712 linux-headers-rpi-v8
  linux-image-rpi-2712 linux-image-rpi-v8 linux-libc-dev locales lp-connection-editor lxplug-netman lxplug-updater mount pcmanfm pi-bluetooth
  piclone pipanel pipewire pipewire-bin pipewire-libcamera pipewire-pulse pishutdown piwiz pixflat-theme python3-libcamera python3-pil
  python3-v4l2 raspberrypi-sys-mods raspberrypi-ui-mods raspi-config raspi-firmware raspi-utils rc-gui realvnc-vnc-server rfkill rp-prefapps
  rpi-eeprom rpi-firefox-mods rpicam-apps util-linux util-linux-extra vlc vlc-bin vlc-data vlc-l10n vlc-plugin-access-extra vlc-plugin-base
  vlc-plugin-notify vlc-plugin-qt vlc-plugin-samba vlc-plugin-skins2 vlc-plugin-video-output vlc-plugin-video-splitter vlc-plugin-visualization
  wayfire wayvnc wf-panel-pi xserver-common xserver-xorg-core
124 upgraded, 25 newly installed, 0 to remove and 0 not upgraded.
```

## 4.2. install opencv
実行中なので、結果の表示は後回しにします。

```bash
# only C++
$ sudo apt-get install libopencv-dev
# need Python also?
$ sudo apt-get install python3-opencv
```

# reference
1. https://qengineering.eu/install-opencv-on-raspberry-pi.html
2. https://ar-ray.hatenablog.com/entry/2021/07/21/101911