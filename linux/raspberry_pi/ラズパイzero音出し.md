# ラズパイzero音出し

* ラズパイzeroからusb-アナログ音声出力変換器+アナログスピーカー経由で音を出す方法
* 検証環境
  * raspberry pi zero HW
  * raspbian OS buster

## 手順

* usb-アナログ音声出力変換器+アナログスピーカー接続
* usbデバイス確認
  * lsusb
    * 出力例
      ```
      # lsusb
      Bus 001 Device 003: ID 0d8c:0014 C-Media Electronics, Inc. Audio Adapter (Unitek Y-247A)
      Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
      ```
* サウンドカード一覧を見て、出力先に指定したいデバイス番号を確認
  * cat /proc/asound/cards
    * 出力例
      ```
      # cat /proc/asound/cards
      0 [b1             ]: bcm2835_hdmi - bcm2835 HDMI 1
                            bcm2835 HDMI 1
      1 [Device         ]: USB-Audio - USB Audio Device
                            C-Media Electronics Inc. USB Audio Device at usb-20980000.usb-1, full speed
      ```
      * この場合USBは1番なので、1番へ出力したい
* サウンド出して確認
  * aplay /usr/share/sounds/alsa/Noise.wav
    * 最初はHDMIの出力優先度が高いので、hdmi接続していなければ音が出ない
* 音声出力優先度変更
  * /usr/share/alsa/alsa.conf
    * 下記のようにパラメータを0=>(指定したいデバイス番号)に変更
      ```
      defaults.ctl.card 1
      defaults.pcm.card 1
      ```
    * 下記はコメントアウト
      ```
      #     "~/.asoundrc"
      ```
* (オプション)mp3再生ソフトインストール
  * apt install -y mpg321
* 任意のmp3ダウンロード
* mp3再生
  * mpg321 kisho.mp3
* (オプション)音量調整
  * alsamixer
    * GUI間隔で音量調整可能