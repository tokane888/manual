# RPZ-IR-Sensor Rev2.0

* 温度／湿度／明度を計測し、赤外線送受信ができる拡張ボード
* 検証環境
  * raspberry Pi zero HW
  * OS: Raspbian GNU/Linux 10 (buster)
* 参考) 公式ガイド？
  * https://www.indoorcorgielec.com/products/rpz-ir-sensor/

## 準備

* apt install -y lirc
* 設定ファイルを下記のようにコメントアウト
  * /boot/config.txt
    ```
    dtparam=i2c_arm=on
    (省略)
    # Uncomment this to enable infrared communication.
    dtoverlay=gpio-ir,gpio_pin=17
    dtoverlay=gpio-ir-tx,gpio_pin=18
    ```
    * /boot/config.txt は他OSのBIOS相当の設定ファイル
      * 再起動するまでは設定変更は反映されない
* 拡張ボードを使用するように設定ファイル編集
  * /etc/lirc/lirc_options.conf
    * driver, device部分を下記のように編集
      ```
      [lircd]
      nodaemon        = False
      driver          = default
      device          = /dev/lirc0
      output          = /var/run/lirc/lircd
      pidfile         = /var/run/lirc/lircd.pid
      plugindir       = /usr/lib/arm-linux-gnueabihf/lirc/plugins
      permission      = 666
      allow-simulate  = No
      repeat-max      = 600
      ```
* i2c有効化
  * raspi-config
  * 3 Interface Options
  * P5 I2C
  * Yes
* i2c関連ツールインストール
  * apt update -y
  * apt install -y python3-smbus python-dev python3-dev i2c-tools
* reboot
* i2cで接続されているデバイス一覧表示して動作確認
  * i2cdetect -y 1
    * 下記のような表示がされることを確認
      ```
      # i2cdetect -y 1
          0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
      00:          -- -- -- -- -- -- -- -- -- -- -- -- --
      10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
      20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
      30: -- -- -- -- -- -- -- -- -- 39 -- -- -- -- -- --
      40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
      50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
      60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
      70: -- -- -- -- -- -- 76 77
      ```
* 公式からLED, センサー、赤外線サンプルプログラム取得
  * 公式) https://www.indoorcorgielec.com/products/rpz-ir-sensor/#%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%A0
    * ダウンロードリンク
      * センサーから各種情報取得
        * https://www.indoorcorgielec.com/wp-content/uploads/products/rpz-ir-sensor/rpz-sensor.zip
      * スイッチ押下中Led4つ光らせる
        * https://www.indoorcorgielec.com/wp-content/uploads/products/rpz-ir-sensor/rpz-ledsw.zip
      * 赤外線
        * https://www.indoorcorgielec.com/resources/raspberry-pi/python-pigpio-infrared/
* サンプル実行時にpython3では下記のエラーが出るので必要なモジュールをインストール
  ```
  $ python3 rpz_ledsw.py
  Traceback (most recent call last):
    File "rpz_ledsw.py", line 2, in <module>
      import RPi.GPIO as GPIO
  ModuleNotFoundError: No module named 'RPi'
  ```
  * apt install -y python3-pip
  * pip3 install rpi.gpio
* センサーから各種情報を取得するサンプルを実行する場合、下記依存モジュールもインストール
  * pip3 install docopt
  * cronに下記のような形で登録しておけば定期実行可能
    ```
    * * * * * /usr/bin/mkdir -p /var/log/sensor/$(/usr/bin/date +\%Y) && /usr/local/bin/lib/rpz-sensor/python3/rpz_sensor.py -l /var/log/sensor/$(/usr/bin/date +\%Y)/$(/usr/bin/date +\%m\%d).log
    ```
* 赤外線センサー
  * サンプル取得
    * git clone https://github.com/IndoorCorgi/cgir.git
  * 必要モジュールインストール及びstart, enable
    * apt install -y pigpio
    * systemctl start pigpiod
    * systemctl enable pigpiod
    * pip3 install pigpiod
    * reboot
  * 赤外線記録
    * ./cgirtool.py send (赤外線名)
      * 例) ./cgirtool.py rec fan_power
  * 赤外線送信
    * ./cgirtool.py send (赤外線名)
      * 例) ./cgirtool.py send fan_power
  * 古い照明など動かないものもあった
  * 赤外線をデコードして書き出し
    * ./cgirtool.py dec -f debug.log (赤外線名)
    