# dnsmasq

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

## 状態確認

* IPリース状況
  * /var/lib/misc/dnsmasq.leases