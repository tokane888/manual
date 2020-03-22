## ubuntuでIPを固定する方法

### 動確環境

ubuntu 18.04実機

### 概要

* ubuntu 16.04までと大分手順が変わっている
  * いじらなくなったファイル
    * /etc/network/interfaces
* netplan, NetworkManagerがネットワーク情報を管理している
  * netplan
    * yamlでネットワーク設定
* 設定ファイル
  * /etc/netplan/*
    * 上記パスの設定ファイルがアルファベット順に読まれ、順次上書きされる
* ログ確認
  * systemctl status NetworkManager

### ルーター側設定(Buffalo前提)

* Buffaloルーターへログイン
* 詳細設定 => LAN => LAN
* "割当IPアドレス"の範囲に、割り当てる固定IPが含まれるように調整して"設定"
* (ルーターが再起動するので)再ログイン
* 詳細設定 => LAN => DHCPリース
* "リース情報の追加"にIP, MACアドレス入力
  * MACアドレスは区切りのコロン(":")も入力

### クライアント側設定

#### ethernetで固定IP

* 下記テンプレの、後述の項目を書き換え
  ```
  # Let NetworkManager manage all devices on this system
  network:
    version: 2
    renderer: NetworkManager
    ethernets:
      enp3s0:
          dhcp4: no
          addresses: [192.168.11.100/24]
          gateway4: 192.168.11.1
          nameservers:
              addresses: [192.168.11.1]
  ```
  * "ethernets"直下のデバイス名
    * ip addr コマンドで確認。上記では"enp3s0"
      * nmcli c コマンドでも確認できるかも
  * addresses: 固定IP
  * gateway4
  * nameservers.addresses
* 書き換え後に下記実行
  * netplan generate
  * netplan apply

#### wifiで固定IP

* 下記テンプレの、後述の項目を書き換え
  ```
  # Let NetworkManager manage all devices on this system
  network:
    version: 2
    renderer: NetworkManager
    wifis:
      wlp2s0:
          dhcp4: no
          addresses: [192.168.11.100/24]
          gateway4: 192.168.11.1
          nameservers:
              addresses: [192.168.11.1]
          access-points:
              (-_-)zz:
                  password: (パスワード)
          
  ```
  * "wifis"直下のデバイス名
    * ip addr コマンドで確認。上記では"wlp2s0"
      * nmcli c コマンドでも確認できるかも
  * addresses: 固定IP
  * gateway4
  * nameservers.addresses
  * access-points配下のSSID+password
* 書き換え後に書き実行
  * netplan generate
  * netplan apply
* 上記実行後、必要ないはずだが一応PC再起動したが、IP固定できなかった
  * だが翌日、ルーターも夜間に自動再起動された後では、ip addr実行したところ、IPは固定されていた
  * TODO: ルーター再起動必要なのか検証