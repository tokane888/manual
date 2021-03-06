# nftables

* 参考
  * https://wiki.archlinux.org/title/nftables

## 概要

* ルーティング、ファイアーウォールなどとして使用される
* iptablesの後継
  * raspbian OS buster: nftablesとiptablesが両方入っている
  * Debian: こちらでも採用されたらしい
* kernelのサポートが必要。サポート有無は下記で確認可能
  * lsmod | grep nf_tables
* iptablesとの互換レイヤがあり、バックエンドとしてnftablesを使用し、フロントはiptablesを使用する事が可能
  * バックエンドとしてnftablesを使用している場合、下記のような表示になる
    ```
    # iptables --version
    iptables v1.8.2 (nf_tables)
    ```
    * この場合、iptables設定ファイルである下記ファイルも存在しない
      * /etc/sysconfig/iptables
      * ルール自体は下記で確認可能
        * iptables-save
  * nftコマンドによるパケットフィルタなどを行う場合はnftablesパッケージのインストールが必要
* table, chainについてはリンク先の図が分かりやすい
  * https://upload.wikimedia.org/wikipedia/commons/3/37/Netfilter-packet-flow.svg

## 関連コマンド

* 導入
  * apt install -y nftables
* 基本コマンド
  * ルールセット全体を表示
    * nft list ruleset
      * パケット量等も何故か出力される
        ```
        root@pi3:/home/tom# nft list ruleset ip
        table ip nat {
                chain PREROUTING {
                        type nat hook prerouting priority -100; policy accept;
                }

                chain INPUT {
                        type nat hook input priority 100; policy accept;
                }

                chain POSTROUTING {
                        type nat hook postrouting priority 100; policy accept;
                        oifname "eth0" counter packets 3324 bytes 210206 masquerade
                }

                chain OUTPUT {
                        type nat hook output priority -100; policy accept;
                }
        }
        ```
  * IPv4に関するルールセット全体を表示
    * nft list ruleset ip
  * ルールセット全体を消去
    * nft flush ruleset
  * IPv4に関するルールセット全体を消去
    * nft flush ruleset ip
* 設定ファイル
  * /etc/nftables.conf
* nat table確認
  * nft list table ip nat
* reload
  * echo "flush ruleset" > /tmp/nftables 
  * nft -s list ruleset >> /tmp/nftables
  * nft -f /tmp/nftables