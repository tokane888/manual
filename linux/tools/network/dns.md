## dns設定

* 動確環境
    * Ubuntu 18.04 (EC2)

### DNSサーバ指定手順

* 設定ファイル編集
    * vim /etc/systemd/resolved.conf
    * DNS=~~
        * DNSサーバ指定
    * FallbackDNS=~~
        * (option)代替DNSサーバ指定
* service再起動
    * systemctl restart systemd-resolved

### Ubuntuに於ける設定ファイル

* /etc/hosts
    * IPとホスト名の組み合わせ
* /etc/resolv.conf -> /run/systemd/resolve/stub-resolv.conf
    * 自動生成
    * DNSサーバ指定(127.0.0.53:53)
        * localのsystemd-resolvedへ問い合わせる
    * systemd以前から使われている設定ファイル
        * 今はsystemd-resolvedへ問い合わせるだけで編集不要
    * シンボリックリンク
        * 下記にリンク先を修正すると、NXDOMAINがどうとかのエラーも消えるとの情報あり
            * /run/systemd/resolve/resolv.conf
            * https://askubuntu.com/questions/1058750/new-alert-keeps-showing-up-server-returned-error-nxdomain-mitigating-potential
* /etc/systemd/resolved.conf
    * DNSサーバ指定
* /etc/nsswitch.conf
    * 名前問い合わせ順序指定
        ```
        hosts:          files mdns4_minimal [NOTFOUND=return] dns myhostname
        ```
        * 左から順に問い合わせ
            * files: /etc/hosts
            * mdns4_minimal: multicast dnsというやつらしい
            * NOTFOUND=return: 見つからない場合、検索停止 
            * dns: dns
            * myhostname: /etc/hosts に自分のhost名を入れなくても良くなるらしい
