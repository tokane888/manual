# ラズパイルーター化手順

## DNS, DHCP等を個別に建てる場合

### 注意点、参考等

* 注意
  * 下記手順で2.4, 5GHzのwifiを飛ばせることはRaspberry Pi 3B+(buster)で確認済み
  * wifiを飛ばす先のchannelを自動的に空いているchannelへ変更する機能はハードウェア的にラズパイのサポート外
  * 干渉を避ける機能が一般的なルータより弱いのか、2.4GHzでは比較的空いているchannelを指定しても安定しなかった
    * ルーターへのpingが1割lost
  * 最大同時接続数は4
    * iw listより
      ```
      * #{ managed } <= 1, #{ P2P-device } <= 1, #{ P2P-client, P2P-GO } <= 1, total <= 3, #channels <= 2
      * #{ managed } <= 1, #{ AP } <= 1, #{ P2P-client } <= 1, #{ P2P-device } <= 1, total <= 4, #channels <= 1
      ```
    * 実際に5デバイスを同時接続したところ、なぜか接続成功
      * メインPCと3デバイスをssh接続した状態で、スマホも接続成功
  * ラズパイルーターは若干低速(設定が微妙なのもあるが)
    * buffaloのルーター使用時: 90Mbps
    * ラズパイルーター使用時: 60Mbps
    * LEDEというルーター専用OSがあるという情報もある
  * 2.4GHzと5GHzのアクセスポイントを同時に1つのraspberry piで提供することは不可
    * 参考) https://raspberrypi.stackexchange.com/questions/103892/can-rpi4-run-simultaneously-on-dual-band-wifi-2-4ghz-5ghz
    * wifiドングルを追加しても無理かは不明瞭
      * 試す場合は下記に注意
        * 当該wifiドングルのlinux向けドライバが公開されていること
        * 電圧が不足しないこと
          * 2.4GHzのみ飛ばせるような低機能なものの方が安定する可能性もある
    * ラズパイルーターの先にbridgeモードのラズパイルーターを置く
* 参考
  * https://www.instructables.com/id/Use-Raspberry-Pi-3-As-Router/
    * WAN側はpppoeではない
  * https://qiita.com/tanaka4410/items/a524cef301f8f7e893c3
    * dnsmasq使っていない
  * https://qiita.com/tanaka4410/items/a524cef301f8f7e893c3
  * https://seasky.blue/weblog/index.php?e=2146
  * http://junyelee.blogspot.com/2019/10/setting-up-raspberry-pi-as-wireless.html
    * dnsmasq+pppoe
* 検証環境
  * raspberry pi 3B+
  * raspbian OS(buster) Lite 32bit
    * Desktop版及び64bit beta版はサポート外とのこと
  * ラズパイ専用電源使用
    * 電源不安定は多くのwifi関連問題の原因になる
* 前提
  * WAN側はpppoe接続
  * 2GHzのwifiのみ提供
    * 5GHz wifiも類似の設定で可能。後述
    * 2GHz, 5GHzの両方のwifiを同時に提供する場合、wifiドングルの購入が必要
* 各種ラズパイのwifi規格
  * 参考) 各規格の詳細については802.11.md参照
  * zero WH
    * 802.11b,g,n
      * 5GHz非対応
  * 3B+
    * 802.11b,g,n,ac
  * 4
    * 802.11b,g,n,ac

## 共通事前準備

* wifi power management をoffにする
  * 現状の設定確認
    * sudo iwconfig
  * 設定
    * sudo iwconfig wlan0 power off
      * 下記のエラーが出て失敗する場合がある
        ```
        # iwconfig wlan0 power off
        Error for wireless request "Set Power Management" (8B2C) :
            SET failed on device wlan0 ; Invalid argument.
        ```
        * 上記エラーが出る場合は下記実行で設定変更
          * iw dev wlan0 set power_save off

## RaspAPを使用する手順

### 検証環境

* raspberry PI 3B+
* OS: raspbian OS Lite buster

### 手順

* 参考) https://github.com/RaspAP/raspap-webgui
* ラズパイsetup.shに従い、wifi接続以外の最低限のセットアップを実施
* 有線LANに接続
* 各パッケージ最新化
```
sudo apt-get -y update
sudo apt-get -y full-upgrade
sudo reboot
```
* インストール
  * sudo su
  * curl -sL https://install.raspap.com | bash
    * 質問が出るが、YでOK
    * 設定を初期状態にリセットしたい場合、再度上記コマンド実行
* デフォルト設定は下記になっている
  ```
  IP address: 10.3.141.1
  Username: admin
  Password: secret
  DHCP range: 10.3.141.50 — 10.3.141.255
  SSID: raspi-webgui
  Password: ChangeMe
  ```
  * 2.4GHz有効化状態
    * 5GHzを有効化する場合は、国設定及びchannel設定が必要
* openvpn無効化
  * systemctl disable 'openvpn-client@client.service'
    * 下記のパスのserviceファイルが削除される
      * /etc/systemd/system/multi-user.target.wants/
* ラズパイセキュリティ.mdに従い、セキュリティ関連設定実施
* IP割当範囲等確認
  * DHCPで割り当てるIPの範囲指定
    * /etc/dnsmasq.d/090_wlan0.conf
      * 下記記載
        * dhcp-host=(mac),(IP)
          * 例) dhcp-host=f0:1d:bc:94:00:0e,10.3.141.3
    * systemctl daemon-reload && systemctl restart dnsmasq
* ブラウザから192.168.11.1へ下記のデフォルトパスワードでアクセスし、認証情報変更
  * user: admin
  * pass: secret

### raspap-webgui関連情報

#### 使用しているパッケージ

* wifi AP提供
  * hostapd
    * 設定
      * /etc/hostapd/hostapd.conf
    * service
      * hostapd
    * 5GHz(hw_mode=a)の状態でchannel=0(自動検索)にしたところhostapdが終了
      * channel=0では自動的に利用者が少ないchannelを検索
        * ラズパイではこの機能はハードウェア的にサポート外とのこと
        * ハードウェア的にサポートしているドングルを接続し、下記のように設定すれば自動で最適なchannelを検索してくれるとの情報も
          * channel=acs_survey
          * 参考) https://www.raspberrypi.org/forums/viewtopic.php?t=180966
      * hw_mode=aで、802.11acの設定になる
        * ラズパイではaは古くてサポートしていない。そのためacになる？
* dnsmasq
  * ログ
    * /tmp/dnsmasq.log
      * ログのパスは下記で指定
        * /etc/dnsmasq.d/090_raspap.conf
* raspap設定ファイル
  * /etc/raspap/
    * 配下にopenvpn, hostapd等の初期化スクリプト的なものがある

#### 速度

* speedtest実行したところ下記の結果になった
  * ルーターとLAN接続したラズパイ上のdown速度
    * 88Mbps
  * ラズパイAPに接続したwindows上のdown速度
    * 42Mbps
  * 市販ルーター配下のラズパイAP(bridge mode)に接続したwindows上のdown速度
    * 46Mbps
  * 市販ルーター配下のラズパイAP(5GHz router)に接続したwindows上のdown速度
    * 59Mbps

### 各パッケージを手動でインストールし、GUIを提供しない手順

* プロバイダ側との接続を確立（プロバイダ.md参照）
* テザリングなどのネット接続可能なネットワークへ接続
* /etc/wpa_supplicant/wpa_supplicant.confのwifiアクセス設定削除
  * アクセスする側になってしまうことを防ぐため
* apt install -y hostapd dnsmasq
* /etc/dhcpcd.conf末尾に下記記載
  ```
  denyinterfaces eth0               # ppp0とデフォルトゲートウェイが重複することを防ぐため。プロバイダ使用時のみ
  interface wlan0
  static ip_address=192.168.11.1/24
  ```
* プロバイダに接続する場合、/etc/network/interfaces に下記の項目が記載されていることを確認
  ```
  auto dsl-provider
  iface dsl-provider inet ppp
  pre-up /bin/ip link set eth0 up
  provider dsl-provider
  ```
  * raspbian OSはjessie以降は/etc/dhcpcd.conf に固定IPを記載する仕様に変更
    * /etc/network/interfaces に固定IPを記載すると、OS起動時にdhcpcd serviceが起動失敗
* hostapd設定
  * /etc/hostapd/wlan0.conf に2ghz向け設定記載(5GHz wifiを飛ばさない場合)
    * ssid, wpa_passphraseは適宜変更
    ```
    interface=wlan0
    driver=nl80211
    country_code=JP
    ssid=pi_2ghz
    hw_mode=g
    channel=7
    wmm_enabled=0
    macaddr_acl=0
    auth_algs=1
    ignore_broadcast_ssid=0
    wpa=2
    wpa_passphrase=(pass)
    wpa_key_mgmt=WPA-PSK
    wpa_pairwise=TKIP
    rsn_pairwise=CCMP
    ```
  * 参考) 5GHz wifiを公開する場合、/etc/hostapd/wlan0.conf に5ghz向け設定記載
    * 2GHzと同時に提供する場合は、wifiドングルの購入が必要
    * ssid, wpa_passphraseは適宜変更
    ```
    driver=nl80211
    ctrl_interface=/var/run/hostapd
    ctrl_interface_group=0
    auth_algs=1
    wpa_key_mgmt=WPA-PSK
    beacon_int=100
    ssid=pi
    channel=48
    hw_mode=a
    ieee80211n=0
    ieee80211ac=1
    wmm_enabled=1
    wpa_passphrase=(password)
    interface=wlan0
    wpa=2
    wpa_pairwise=CCMP
    country_code=JP
    ignore_broadcast_ssid=0
    ```
      * channelの値は、windowsのwifi analyzer等で周囲のwifiの状況を見て判断
        * channel自動選択, 52, 56はラズパイ3B+のオンボードwifiのサポート外
  * systemctl unmask hostapd
  * systemctl enable hostapd
  * 参考) /etc/default/hostapd のDAEMON_CONFでhostapdの設定ファイルパスを指定する方法はdeprecated
    * 最新の方法は　/usr/share/doc/hostapd/README.Debian 参照
* dnsmasq設定
  * /etc/dnsmasq.d/common.conf に下記記載
    ```
    log-facility=/tmp/dnsmasq.log
    conf-dir=/etc/dnsmasq.d
    log-dhcp
    log-queries
    ```
  * /etc/dnsmasq.d/wlan0.conf に下記記載
    * static IPは必要なければ削除
    ```
    interface=wlan0
    dhcp-range=10.168.11.2,10.168.11.64,255.255.255.0,12h

    # static IP
    # surface laptop pro 3
    dhcp-host=f0:1d:bc:**:**:**,10.168.11.101
    # raspberry pi4
    dhcp-host=dc:a6:32:**:**:**,10.168.11.103
    ```
  * 下記で設定ファイルテスト
    * dnsmasq --test
* IPv4のforward設定追加
  * 下記を新規作成し、packet forwarding有効化
    * /etc/sysctl.d/router.conf
      * 下記記載
        * net.ipv4.ip_forward=1
  * iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
  * iptables-saveでnat設定を確認
    ```
    tom@pi4:~# iptables-save
    # Generated by xtables-save v1.8.2 on Sat Jun 12 15:14:27 2021
    *nat
    :PREROUTING ACCEPT [0:0]
    :INPUT ACCEPT [0:0]
    :POSTROUTING ACCEPT [0:0]
    :OUTPUT ACCEPT [0:0]
    -A POSTROUTING -o eth0 -j MASQUERADE
    COMMIT
    # Completed on Sat Jun 12 15:14:27 2021
    ```
  * nat設定をOS再起動後も有効に
    * iptables-save > /etc/iptables.ipv4.nat
  * /etc/rc.local のexit 0直前に下記追記
    * sudo iptables-restore < /etc/iptables.ipv4.nat

## ラズパイ配下の有線portで別subnetのIPをDHCPで提供する手順

* 当該network interfaceに割り当てるIPを設定
  * /etc/dhcpcd.conf
    * 設定例
      ```
      interface wlan0
      static ip_address=10.168.11.1/24
      interface eth1
      static ip_address=10.168.12.1/24
      ```
      * 無線のIPと同一subnetにするとroutingの問題で正常にip割当が行われないので注意
* 提供するsubnetをdnsmasqで指定
  * /etc/dnsmasq.d/router.conf
    * 設定
      ```
      interface=wlan0
      dhcp-range=10.168.11.2,10.168.11.64,255.255.255.0,12h
      interface=eth1
      dhcp-range=10.168.12.2,10.168.12.64,255.255.255.0,12h
      ```

## port forward設定

* 下記で一時的に設定可能
  * iptables -t nat -A PREROUTING -p tcp -d 123.217.81.218 --dport 40022 -j DNAT --to-destination 192.168.12.100:22
  * ただDDNSだとルーター再起動時に外側IPが変更されるため、対策が必要
    * interface指定？
    * TODO: 対策方法調査

## 2.4GHz同時提供用にusbドングル追加する手順

* 注意
  * 検証中手順。現状成功していない
* 検証環境
  * 筐体: raspberry pi 3B+
  * OS: raspbian OS Buster
  * usbドングル: Realtek 0xc811

### セットアップ手順

* usbドングルの種類確認
  * usbドングルをラズパイに指す
  * lsusb
    ```
    # lsusb
    Bus 001 Device 006: ID 0bda:c811 Realtek Semiconductor Corp.
    Bus 001 Device 005: ID 0424:7800 Standard Microsystems Corp.
    Bus 001 Device 003: ID 0424:2514 Standard Microsystems Corp. USB 2.0 Hub
    Bus 001 Device 002: ID 0424:2514 Standard Microsystems Corp. USB 2.0 Hub
    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    ```
    * 最もDevice番号の大きいものが一番あとに指したもの
  * 詳細確認
    * lsusb -s 001:006 -v
      * 実行結果例抜粋
        ```
        idVendor           0x0bda Realtek Semiconductor Corp.
        idProduct          0xc811
        ```
  * 当該usbのドライバをchrome等で検索