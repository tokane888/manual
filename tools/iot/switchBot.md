# switch bot

* ラズパイからswitch botにボタンを押させる
* ライブラリ導入環境
  * raspberry pi 3B+
    * zeroは最後のgattlibのインストールが結局成功しなかった

## 導入失敗したライブラリ

* 公式ライブラリ
  * https://github.com/OpenWonderLabs/python-host#python-3-and-new-bluetooth-stack-support

### 導入

```
sudo apt-get install -y python3-pip libbluetooth-dev
pip3 install pybluez
sudo apt-get install -y libboost-python-dev libboost-thread-dev
```

* 下記実行でgattlibインストール
```
nohup pip3 install gattlib &
```
  * 注意) ラズパイzeroだとインストールも後述のコンパイルもメモリ不足で下記エラーで失敗
    * arm-linux-gnueabihf-gcc: fatal error: Killed signal terminated program cc1plus
    * zeroの場合は一度下記実行でswapを2GB程度に設定の上rebootしてからビルド
      * sudo sed -i -e "s/^CONF_SWAPSIZE=.*/CONF_SWAPSIZE=2048/g" /etc/dphys-swapfile
* 下記箇所の負荷が高くsshがハングアップする場合があるのでnohupを使用している
  ```
  Building wheels for collected packages: gattlib
    Running setup.py bdist_wheel for gattlib ... /
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

### 各種コマンド

* ボタン押下
  * python3 switchbot_py3.py -d xx:xx:xx:xx:xx:xx -c on
    * xx:xx:...はmac address
* ボタン引く
  * python3 switchbot_py3.py -d xx:xx:xx:xx:xx:xx -c off