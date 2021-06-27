# iptables

## 概要

* 2021年現在主流のfirewall, packet filteringツール
* 後継のnftablesへの置き換えが徐々に始まっている
* 各種ルールの優先度は下記
  1. デフォルトルールは優先度最低
  2. 前の方に書かれたルールは後の方のルールより有線される

## コマンド

* ルール一覧表示
  * iptables -L
    ```
    # iptables -L
    Chain INPUT (policy ACCEPT)
    target     prot opt source               destination
    DROP       tcp  --  anywhere             anywhere             tcp dpt:domain
    DROP       udp  --  anywhere             anywhere             udp dpt:domain
    ACCEPT     udp  --  anywhere             10.168.11.1          udp dpt:domain

    Chain FORWARD (policy ACCEPT)
    target     prot opt source               destination

    Chain OUTPUT (policy ACCEPT)
    target     prot opt source               destination
    ```
    * chain指定での絞り込みも可能
      * iptables -L INPUT
* ルール削除
  * 単一のルール削除
    * 削除したいルールのindex確認
      * iptables -L INPUT --line-numbers -n
        ```
        # iptables -L INPUT --line-numbers -n
        Chain INPUT (policy ACCEPT)
        num  target     prot opt source               destination
        1    DROP       tcp  --  anywhere             anywhere             tcp dpt:domain
        2    DROP       udp  --  anywhere             anywhere             udp dpt:domain
        3    ACCEPT     udp  --  anywhere             10.168.11.1          udp dpt:domain
        ```
      * -n: 表中のipをドメインに変換しない
    * chain名、index指定で削除
      * iptables -D INPUT 1
  * 特定chainのルール全削除
    * iptables -F INPUT
* 設定例
  * 宛先が特定IPのパケットをブロック
    * 133.152.43.29(www.nicovideo.jp)をブロックする場合
      * iptables -A FORWARD -d 133.152.43.29 -j DROP
  * 外部DNSサーバへの問い合わせをブロック
    * iptables -A FORWARD -p udp --dport 53 -j DROP
      * udpは問い合わせ。tcpはゾーン転送等
* ログ出力
  * iptablesコマンド末尾に下記記載で出力される
    * -j LOG
  * ログを吐くのはkernelなので、下記に他のログを混じって出力される
    * /var/log/kern.log
  * 見分けるためにprefixをつける場合、iptablesコマンド末尾に下記記載
    * -j LOG --log-prefix='[iptables-53] '
* 設定保存
  * デフォルトではOS再起動で設定はリセットされる
  * 新規パッケージをインストールしないで設定保存する方法
    * 設定保存
      * iptables-save > /etc/iptables.ipv4.nat
    * OS起動時処理で、保存した設定を読み込む用に設定
      * /etc/rc.local
        * 下記追記
          * sudo iptables-restore < /etc/iptables.ipv4.nat
  * 専用パッケージを使用する方法
    * apt install -y iptables-persistent
      * 確認ダイアログが出るのでYes選択
    * ルール保存時に下記実行
      * netfilter-persistent save
        * OS再起動時には自動で保存した設定が読み込まれる
  