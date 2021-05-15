# dnsmasq

* dnsキャッシュサーバ兼dhcpサーバ
  * dns権威サーバはサポート外の模様

## 設定

* 設定ファイル
  * /etc/dnsmasq.conf
  * /etc/default/dnsmasq
    * こちらはほとんどのケースで触らなくて良いとのこと
* dnsmasqへのクエリの転送先dnsサーバー
  * systemdのresolvconfを使用している場合
    * dnsmasqのクエリ転送先は下記に自動で記載される
      * /var/run/dnsmasq/resolv.conf
    * 下記設定ファイルのnameserverが127.0.0.1になっていればresolvconfを使用している
        * /etc/resolv.conf* resolvconfを使用している場合
  * resolvconfがなく、dhcpcdを使用している場合
    * dnsmasqのクエリ転送先は下記に自動で記載される
      * /etc/dhcpc/resolv.conf
  * resolvconfがなく、pppdを使用している
    * dnsmasqのクエリ転送先は下記に自動で記載される
      * /etc/ppp/resolv.conf
* resolveconfを使用していない場合
  * 下記設定ファイルのdns-nameserver指定は無視される
    * /etc/network/interfaces
  * resolveconfを使用しない場合、/etc/resolv.confの最初の行に127.0.0.1を記載
  * /etc/dnsmasq.confにserver=(IP)の形式でdnsサーバを指定

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