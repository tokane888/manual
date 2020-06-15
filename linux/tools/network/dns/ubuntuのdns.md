# ubuntuのdns

* ubuntu 16.10以降、ローカルdnsリゾルバーが変更されている
  * dnsmasq => systemd-resolved
    * systemd-resolvedはd-bus不要、DNSSECサポート

## 新ローカルdnsリゾルバー

* /etc/resolve.conf
  * 127.0.0.53:53
    * ここは元々キャッシュを持たなかった