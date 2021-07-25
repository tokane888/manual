# iptables

## 各種コマンド

* ルール確認
  * INPUT, OUTPUT, FORWARD chain及び処理済みpacket数表示
    * iptables -L --line-number -v
  * 上記以外のchain及び処理済みpacket数表示
    * iptables -L --line-number -t nat -v
      * 表示例
        ```
        # iptables -L --line-number -t nat -v
        Chain PREROUTING (policy ACCEPT 702 packets, 79943 bytes)
        num   pkts bytes target     prot opt in     out     source               destination
        1        0     0 DNAT       tcp  --  eth0   any     anywhere             anywhere             tcp dpt:40022 to:192.168.12.100:22
        2        1    60 DNAT       tcp  --  ppp0   any     anywhere             anywhere             tcp dpt:40022 to:192.168.12.100:22

        Chain INPUT (policy ACCEPT 246 packets, 20902 bytes)
        num   pkts bytes target     prot opt in     out     source               destination

        Chain POSTROUTING (policy ACCEPT 0 packets, 0 bytes)
        num   pkts bytes target     prot opt in     out     source               destination
        1      530 64061 MASQUERADE  all  --  any    any     anywhere             anywhere
        2        0     0 MASQUERADE  all  --  any    any     192.168.50.0/24     !192.168.50.0/24

        Chain OUTPUT (policy ACCEPT 163 packets, 12653 bytes)
        num   pkts bytes target     prot opt in     out     source               destination
        ```
  * PREROUTING設定確認方法
    * iptables -L -n -t nat
      * 通常はPREROUTINGのnat tableは表示しないため、iptables -Lでは表示されない
        * 上記のように明示的に指定すると表示される
* 番号指定でルール削除
  * iptables -D (chain) (num) [-t (table名)]
    * 例) iptables -D POSTROUTING 2 -t nat
* 具体例
  * 外部dns(ppp interface側)への名前解決パケット(udpの53portへのパケット)のforwardをDROPするルール追加
    * iptables -t filter -A FORWARD -p udp --dport 53 -o ppp0 -j DROP
  * interface指定でのport forward
    * iptables -t nat -A PREROUTING -p tcp -i ppp0 --dport 40022 -j DNAT --to-destination 192.168.12.100:22
  * IP指定でのport forward
    * iptables -t nat -A PREROUTING -p tcp -d 123.217.81.218 --dport 40022 -j DNAT --to-destination 192.168.12.100:22
    * ただDDNSだとルーター再起動時に外側IPが変更されるため、その場合は上記のinterface指定する方法を使用
  * 内部ネットワークの192.168.12.103の80番portへ、40080portからアクセス可能に
    * iptables -t nat -A PREROUTING -p tcp -i ppp0 --dport 40080 -j DNAT --to-destination 192.168.12.103:80
  * 特定macアドレスの全パケットを遮断
    * iptables -A INPUT -m mac --mac-source F0:1D:BC:AA:BB:CC -j DROP
      * ルールを削除する場合は、-A => -D に置き換えるだけ
        * iptables -D INPUT -m mac --mac-source F0:1D:BC:AA:BB:CC -j DROP
  * 特定macアドレスの全パケットFORWARDを許可
    * iptables -I FORWARD 1 -m mac --mac-source 48:F1:7F:AA:BB:CC -j ACCEPT
  * 特定の宛先への全パケットを遮断
    * iptables -A FORWARD -d 142.250.196.110 -j DROP

## 設定永続化方法

* 方法1: 設定永続化のため、iptables-persistentパッケージインストール
  * apt install -y iptables-persistent
    * ダイアログが出るので、現在のルールを保存を選択
    * 今後新規ルール追加時は下記で保存可能
      * netfilter-persistent save
  * 設定ファイルは下記に出力される
    * /etc/iptables/rules.v4
    * /etc/iptables/rules.v6
