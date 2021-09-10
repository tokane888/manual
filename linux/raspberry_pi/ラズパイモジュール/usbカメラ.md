# usbカメラ接続

* 専用モジュールではないが。。
* 検証環境
  * Logitech, Inc. Webcam C270
  * Raspberry pi 3B+
  * Raspbian OS buster

## 静止画撮影手順

* カメラをラズパイにusb接続
* 下記実行
  ```
  sudo apt-get install fswebcam
  fswebcam -D 5 -r 1280x720 -S 100 --no-title --no-banner --no-shadow ./test.jpg
  ```
