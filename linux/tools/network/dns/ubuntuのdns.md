# ubuntuのdns

* ubuntu 16.10以降、ローカルdnsリゾルバーが変更されている
  * dnsmasq => systemd-resolved

## 新ローカルdnsリゾルバー

* /etc/resolve.conf
  * 127.0.0.53:53
    * systemd-resolvedへの問い合わせになる
* systemd-resolved
  * 機能的にはlocalで動くキャッシュ機能を持つだけのresolver

## ubuntu 18.04でdig +traceが失敗する問題

* 原因
  * dig +norecurse . NS をsystemd-resolvedがREFUSEDにすることが原因
    * https://github.com/systemd/systemd/issues/5897
* 暫定対応
    * /etc/resolv.conf のDNS指定を 127.0.0.53 => 8.8.8.8 に変更する等の対応が可能
    * 下記記載の各dnsサーバに対して、直接digで問い合わせる
      * /etc/netplan/***.yaml
      * /etc/systemd/resolved.conf
      * 問い合わせ例
        * dig +trace (FQDN)
* 恒久対応
  * 参考) 最新verではdig側対応で修正済み。ただデフォルトのdeb package repositoryには入っていない
    * 現状最新版を取得する方法は不明
      * 下記から取得できそうだったが、Debian OS向けだからなのか、dnsutilsに実行ファイルが入っていない
        * https://pkgs.org/download/dnsutils
  * 下記でdrillをインストール
    * apt install -y ldnsutils
  * drillでtrace
    * drill -T (FQDN)
      * レスポンスを送ってきたdnsがどれなのか分からないのが問題
```
root@ip-172-31-13-53:~# drill -T www.google.com
.       518400  IN      NS      e.root-servers.net.
.       518400  IN      NS      h.root-servers.net.
.       518400  IN      NS      l.root-servers.net.
.       518400  IN      NS      i.root-servers.net.
.       518400  IN      NS      a.root-servers.net.
.       518400  IN      NS      d.root-servers.net.
.       518400  IN      NS      c.root-servers.net.
.       518400  IN      NS      b.root-servers.net.
.       518400  IN      NS      j.root-servers.net.
.       518400  IN      NS      k.root-servers.net.
.       518400  IN      NS      g.root-servers.net.
.       518400  IN      NS      m.root-servers.net.
.       518400  IN      NS      f.root-servers.net.
com.    172800  IN      NS      a.gtld-servers.net.
com.    172800  IN      NS      b.gtld-servers.net.
com.    172800  IN      NS      c.gtld-servers.net.
com.    172800  IN      NS      d.gtld-servers.net.
com.    172800  IN      NS      e.gtld-servers.net.
com.    172800  IN      NS      f.gtld-servers.net.
com.    172800  IN      NS      g.gtld-servers.net.
com.    172800  IN      NS      h.gtld-servers.net.
com.    172800  IN      NS      i.gtld-servers.net.
com.    172800  IN      NS      j.gtld-servers.net.
com.    172800  IN      NS      k.gtld-servers.net.
com.    172800  IN      NS      l.gtld-servers.net.
com.    172800  IN      NS      m.gtld-servers.net.
google.com.     172800  IN      NS      ns2.google.com.
google.com.     172800  IN      NS      ns1.google.com.
google.com.     172800  IN      NS      ns3.google.com.
google.com.     172800  IN      NS      ns4.google.com.
www.google.com. 300     IN      A       172.217.24.132
```