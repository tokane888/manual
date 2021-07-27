# netperf

* server-client間の速度計測
* iperfと類似のパッケージ

## 導入

* raspbian OSの場合
  * deb repositoryで提供されていないため、下記でビルド
    ```
    apt update -y && apt install -y python-pip
    apt install -y autotools-dev automake
    git clone https://github.com/HewlettPackard/netperf.git
    cd netperf
    ./autogen.sh
    ./configure --enable-demo
    make
    sudo make install
    sudo pip install flent
    sudo pip install matplotlib
    ```
* ubuntuの場合
  * apt install -y netperf

## 計測

* サーバ側
  * サーバが自動で起動されていること確認
    * ps aux | grep netserver
      * 起動されていない場合手動で起動
        * netserver
* client側
  * netperf -H (サーバIP)
    * 出力例
      ```
      # netperf -H 192.168.12.103
      MIGRATED TCP STREAM TEST from 0.0.0.0 () port 0 AF_INET to 192.168.12.103 () port 0 AF_INET : demo
      Recv   Send    Send
      Socket Socket  Message  Elapsed
      Size   Size    Size     Time     Throughput
      bytes  bytes   bytes    secs.    10^6bits/sec

      131072  16384  16384    10.36      67.82
      ```
