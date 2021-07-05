## momo

### raspberry pi 4B+からwebrtcで映像配信

#### raspberry pi 4B+環境構築

* 参考) https://github.com/shiguredo/momo/blob/develop/doc/SETUP_RASPBERRY_PI.md
* momoバイナリダウンロードし、raspberry piへ転送
  * ダウンロードurl
    * https://github.com/shiguredo/momo/releases
  * ダウンロードファイル名
    * momo-<VERSION>_raspberry-pi-os_armv7.tar.gz
* raspberry pi上で解凍
  * tar -zxvf momo-<VERSION>_raspberry-pi-os_armv7.tar.gz
  * cd momo-<VERSION>_raspberry-pi-os_armv7
* パッケージダウンロード
  ```
  sudo apt update -y
  sudo apt install -y libnspr4 libnss3
  ```
* raspi-configでcameraをenable
* カーネルモジュール設定
  * sudo modprobe bcm2835-v4l2 max_video_width=2592 max_video_height=1944
* 他依存パッケージインストール
  * libsdl2-mixer-2.0-0 libsdl2-image-2.0-0 libsdl2-2.0-0
    * マニュアルには記載がなかったがこれがないとmomo実行時に下記の警告が出た
      * ./momo: error while loading shared libraries: libSDL2-2.0.so.0: cannot open shared object file: No such file or directory
* テスト配信
  * ./momo --no-audio-device test
* 他PCから下記で視聴可能
  * http://(raspberry piのip):8080/html/test.html