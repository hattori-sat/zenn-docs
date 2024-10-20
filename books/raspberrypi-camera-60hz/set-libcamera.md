---
title: "libcameraの設定"
free: true
---
# 前置き
前章で説明した**picamera2**を用いれば、pythonで写真を撮るプログラムを作成することができます。しかし、実際に試してみるとわかりますが、実行がかなり遅いです。

おそらく実行速度を早くする書き方はありますが、そこに労力を使うよりもCやC++を用いてプログラムを書いた方が多くの人において実行速度は早くなると思います。（単にインタプリタよりもコンパイルされるCの方が早いだろうという考えです。）

調べるとC++でcamera moduleを用いてcaptureしている記事は確認することができますが、コード付きで紹介してくれている記事は少なかったです。

いくつかC++で使えそうなライブラリがありました。raspberypiのUserlandにもその機能がありますし、OpenCVにもcaptureできるものもあります。撮影後にOpenCVで画像処理をしたいことが多いと思いますので、OpenCVを使った方が良い可能性もありますが、どうやら少し速度が遅いようです。

そんな感じで探していったところ公式にlibcameraというC++で書かれたAPIを見つけました。今回は、raspberryPiのlibcameraを用いることにしました。

# libcameraのセットアップ
[公式のgithub](https://github.com/raspberrypi/libcamera?tab=readme-ov-file#libcamera)のREADME.mdに記載の内容に従ってインストールをしていきます。

## 依存関係のインストール
```bash
sudo apt update
sudo apt install -y python3-pip git python3-jinja2
sudo apt install -y libboost-dev
sudo apt install -y libgnutls28-dev openssl libtiff-dev pybind11-dev
sudo apt install -y qtbase5-dev libqt5core5a
sudo apt install -y meson cmake
sudo apt install -y python3-yaml python3-ply
sudo apt install -y libglib2.0-dev libgstreamer-plugins-base1.0-dev
```
##  libcameraのダウンロードとビルド

```bash
git clone https://github.com/raspberrypi/libcamera.git
cd libcamera
meson setup build --buildtype=release -Dpipelines=rpi/vc4,rpi/pisp -Dipas=rpi/vc4,rpi/pisp -Dv4l2=true -Dgstreamer=enabled -Dtest=false -Dlc-compliance=disabled -Dcam=disabled -Dqcam=disabled -Ddocumentation=disabled -Dpycamera=enabled
ninja -C build install
```

とりあえずコンパイルするためにインクルードパスとリンクライブラリが必要です。

```bash
pkg-config --cflags libcamera
pkg-config --libs libcamera
```

私の場合はこんな感じでした。

```bash
hattori@raspi:~ $ pkg-config --cflags libcamera
-I/usr/include/libcamera 
hattori@raspi:~ $ ls /usr/include/libcamera
libcamera
hattori@raspi:~ $ pkg-config --libs libcamera
-lcamera -lcamera-base 
```

これを元にg++は以下のコマンドでコンパイルできます。

```bash
g++ -o capture capture.cpp -I/usr/include/libcamera -lcamera -lcamera-base
```

ただし、肝心なcppコードがまだできていない。以下はまだ動かない。

```cpp
#include <libcamera/libcamera.h>
#include <libcamera/camera_manager.h>
#include <libcamera/camera.h>
#include <libcamera/framebuffer_allocator.h>
#include <libcamera/request.h>
#include <iostream>
#include <memory>

using namespace libcamera;

int main() {
    CameraManager cm;
    cm.start();

    std::shared_ptr<Camera> camera = cm.get("imx219");
    if (!camera) {
        std::cerr << "Camera not found!" << std::endl;
        return -1;
    }

    camera->acquire();

    std::unique_ptr<CameraConfiguration> config = camera->generateConfiguration({ StreamRole::StillCapture });
    config->at(0).pixelFormat = libcamera::formats::RGB888;
    config->at(0).size = { 640, 480 };

    camera->configure(config.get());

    FrameBufferAllocator allocator(camera);
    for (auto &stream : *config) {  // 修正: streams() メソッドの代わりに config を直接使用
        allocator.allocate(stream);
    }

    std::unique_ptr<Request> request = camera->createRequest();  // 修正: std::unique_ptr を使用
    if (!request) {
        std::cerr << "Failed to create request!" << std::endl;
        return -1;
    }

    for (auto &stream : *config) {  // 修正: streams() メソッドの代わりに config を直接使用
        FrameBuffer *buffer = allocator.buffers(&stream).front().get();
        request->addBuffer(&stream, buffer);
    }

    camera->start();
    camera->queueRequest(request.get());  // 修正: std::unique_ptr を使って request を渡す

    cm.stop();

    return 0;
}
```