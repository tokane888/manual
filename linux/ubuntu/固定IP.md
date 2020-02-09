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
  * NetworkManager
    * netplanが使ってるrenderer？らしい
    * netplanから呼び出されて使用される
    * nmcli helpでヘルプ表示
* 設定ファイル
  * /etc/netplan/*
    * 上記パスの設定ファイルがアルファベット順に読まれ、順次上書きされる
* ログ確認
  * systemctl status NetworkManager

### ethernetで固定IP

* 下記テンプレの、後述の項目を書き換え
  ```
  # Let NetworkManager manage all devices on this system
  network:
    version: 2
    renderer: NetworkManager

    ethernets:
      enp3s0:
          dhcp4: n
          addresses: [192.168.11.100/24]
          gateway4: 192.168.11.1
          nameservers:
              addresses: [192.168.11.1]
          dhcp6: n
  ```
  * "ethernets"直下のデバイス名
    * ip addr コマンドで確認。上記では"enp3s0"
      * nmcli c コマンドでも確認できるかも
  * addresses: 固定IP
  * gateway4
  * nameservers.addresses

  ### wifiで固定IP

* 下記テンプレの、後述の項目を書き換え
  ```
  # Let NetworkManager manage all devices on this system
  network:
    version: 2
    renderer: NetworkManager

    wifis:
      enp3s0:
          dhcp4: n
          addresses: [192.168.11.100/24]
          gateway4: 192.168.11.1
          nameservers:
              addresses: [192.168.11.1]
          dhcp6: n
  ```
  * "ethernets"直下のデバイス名
    * ip addr コマンドで確認。上記では"enp3s0"
      * nmcli c コマンドでも確認できるかも
  * addresses: 固定IP
  * gateway4
  * nameservers.addresses