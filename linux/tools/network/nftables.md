# nftables

* 参考
  * https://wiki.archlinux.org/title/nftables
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
  * nftコマンドによるパケットフィルタなどを行う場合はnftablesパッケージのインストールが必要
* 基本コマンド
  * ルールセット全体を表示
    * nft list ruleset
  * IPv4に関するルールセット全体を表示
    * nft list ruleset ip
  * ルールセット全体を消去
    * nft flush ruleset
  * IPv4に関するルールセット全体を消去
    * nft flush ruleset ip
* 設定ファイル
  * /etc/nftables.conf
* コマンド
  * nat table確認
    * nft list table ip nat