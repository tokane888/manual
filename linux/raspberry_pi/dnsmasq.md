# dnsmasq

* dnsキャッシュサーバ兼dhcpサーバ
  * dns権威サーバはサポート外の模様

## 設定

* 設定ファイル
  * /etc/dnsmasq.conf
  * /etc/default/dnsmasq
    * こちらはほとんどのケースで触らなくて良いとのこと
  * /etc/dnsmasq.d/*
    * 090_wlan0.conf
      * DHCP割当設定
      * RaspAP導入ラズパイで自動生成される
      * 例)
        ```
        # RaspAP wlan0 configuration
        interface=wlan0
        dhcp-range=10.3.141.50,10.3.141.255,255.255.255.0,12h
        ```
    * 090_raspap.conf
      * ログ、AP関連設定
      * RaspAP導入ラズパイで自動生成される
      * 例)
        ```
        # RaspAP default config
        log-facility=/tmp/dnsmasq.log
        conf-dir=/etc/dnsmasq.d
        log-dhcp
        log-queries
        ```
        * dnsクエリ関連は下記へログ出力されている
          * /tmp/dnsmasq.log
* dnsmasqへのクエリの転送先dnsサーバー
  * systemdのresolvconfを使用している場合
    * dnsmasqのクエリ転送先は下記に自動で記載される
      * /var/run/dnsmasq/resolv.conf
    * 下記設定ファイルのnameserverが127.0.0.1になっていればresolvconfを使用している
        * /etc/resolv.conf* resolvconfを使用している場合
  * resolvconfがなく、dhcpcdを使用している場合
    * dnsmasqのクエリ転送先は下記に自動で記載される
      * /etc/dhcpcd/resolv.conf
  * resolvconfがなく、pppdを使用している
    * dnsmasqのクエリ転送先は下記に自動で記載される
      * /etc/ppp/resolv.conf
* resolveconfを使用していない場合
  * 下記設定ファイルのdns-nameserver指定は無視される
    * /etc/network/interfaces
  * resolveconfを使用しない場合、/etc/resolv.confの最初の行に127.0.0.1を記載
  * /etc/dnsmasq.confにserver=(IP)の形式でdnsサーバを指定
* dhcpサーバとしてのみ使用し、dnsサーバ機能を使用しない方法
  * /etc/dnsmasq.d/*.conf に下記のようにdhcp関連設定だけを記載
    ```
    # RaspAP wlan0 configuration
    interface=wlan0
    dhcp-range=10.3.141.50,10.3.141.255,255.255.255.0,12h
    ```
* ip割当を、有効期限前にリセットする方法
  * 下記から当該ipに関する設定行を削除
    * /var/lib/misc/dnsmasq.leases
  * systemctl restart dnsmasq
  * ip割り当てられる側のOS再起動

## サイトブロック方法

* /etc/dnsmasq.hosts に下記形式でブロックするサイト記載
  ```
  address=/youtube.com/
  address=/hoge.com/
  ```
* 上記へシンボリックリンクを貼る
  * cd /etc/dnsmasq.d
  * ln -s /etc/dnsmasq.hosts hosts
* dnsmasq再起動
  * systemctl daemon-reload
  * systemctl restart dnsmasq

## 状態確認

* IPリース状況
  * /var/lib/misc/dnsmasq.leases

## 動作

* DNS query forwarder
* /etc/resolv.confを見て、dns query問い合わせ先のDNSサーバを決める
* SIGHUP受信時
  * キャッシュをクリアし、/etc/hosts, /etc/ethers, --dhcp-*** option等で指定されたファイルを再読み込み
* SIGUSR1受信時
  * system logに統計出力
    * キャッシュサイズ
    * キャッシュから削除されるドメインの数

## トラブルシュート

* ログ出力場所
  * ラズパイの場合
    * /var/lib/misc/dnsmasq.leases
      * 例
        ```
        # tail -f /var/lib/misc/dnsmasq.leases
        1626123076 0c:8b:fd:ab:e9:8d 10.168.11.102 old *
        ```
  * Ubuntuの場合(未確認)
    * /var/lib/dhcp/dhcpd.leases
* journalctl status dnsmasqで出ている下記のwarningは詳細不明だが一旦無視して良さそう
  * May 15 19:27:39 raspberrypi dnsmasq[918]: Too few arguments.
  * 参考) https://raspberrypi.stackexchange.com/questions/120338/dnsmasq-error-dnsmasq1122-too-few-arguments

## Ubuntu 22.04への導入手順

* 53 portを使用しているsystemd-resolved停止
  * systemctl disable --now systemd-resolved
* apt -y install dnsmasq
* /etc/dnsmasq.conf 末尾に下記追記
  ```
  domain-needed
  bogus-priv

  # ログの出力先
  log-facility=/var/log/dnsmasq/dnsmasq.log

  # DNSクエリのログを取る
  log-queries
  ```
* mkdir /var/log/dnsmasq
* ログローテーション設定追加
  * 
* systemd-resolvedがリンクしていた設定ファイルをunlink
  * unlink /etc/resolv.conf
* /etc/resolv.conf のnameserver指定を下記に変更
  ```
  nameserver 127.0.0.1
  nameserver 8.8.8.8
  ```
* 設定反映のため再起動
  * systemctl restart dnsmasq
* dig hoge.com で正常に名前解決出来ること確認