# switch bot

* ラズパイからswitch botにボタンを押させる
* 公式ライブラリ
  * https://github.com/OpenWonderLabs/python-host#python-3-and-new-bluetooth-stack-support
* ライブラリ導入環境
  * raspberry pi 3B+
    * zeroは最後のgattlibのインストールが結局成功しなかった

## 導入

```
sudo apt-get install -y python3-pip libbluetooth-dev
pip3 install pybluez
sudo apt-get install -y libboost-python-dev libboost-thread-dev
```

* 下記実行でgattlibインストール
```
pip3 install gattlib
```

* 上記手順最後のgattlibのインストールが失敗した場合は下記
  * raspberry pi zeroで試したところ実際失敗した
  ```
  sudo apt-get install -y pkg-config python3-dev libglib2.0-dev
  pip3 download gattlib
  tar xvzf ./gattlib-0.20150805.tar.gz
  cd gattlib-0.20150805/
  ```
  
  * setup.pyの中を見てpythonバージョン番号書き換え
    * インストール済みのpython3のversionは下記で確認  
      * python3 --version
    * setup.pyの41行目付近のpython3のパス設定を下記のように実在のパスに書き換え
      * libboost_python3-py37.so (libc6,hard-float) => /usr/lib/arm-linux-gnueabihf/libboost_python3-py37.so
        * "=>"手前のファイル名も実在のものに書き換え
    * 上記で記載したファイルが存在すること確認
  * pip3 install .
* git clone https://github.com/OpenWonderLabs/python-host.git
* cd python-host/

## 各種コマンド

* ボタン押下
  * python3 switchbot_py3.py -d xx:xx:xx:xx:xx:xx -c close
    * xx:xx:...はmac address
    * TODO: 現状下記のエラーになるので原因調査
      * Bluetoothは有効化されていることをhciconfigで確認済み
      * BLEで電源タップのon/offの制御はできているので、OSの設定は問題なさそう
      ```
      # python3 switchbot_py3.py -d D7:B8:08:24:0B:30 -c close
      Connected!
      Traceback (most recent call last):
        File "switchbot_py3.py", line 178, in <module>
          main()
        File "switchbot_py3.py", line 173, in main
          driver.run_command(opts.command)
        File "switchbot_py3.py", line 122, in run_command
          return req.write_by_handle(self.handles[command], self.commands[command])
      gattlib.GATTException: Characteristic value/descriptor operation failed: Attribute can't be written
      ```