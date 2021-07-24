# iptables

* interface指定でのport forward
  * iptables -t nat -A PREROUTING -p tcp -i eth0 --dport 40022 -j DNAT --to-destination 192.168.12.100:22
    * ppp0の方が良いかもしれない
* IP指定でのport forward
  * iptables -t nat -A PREROUTING -p tcp -d 123.217.81.218 --dport 40022 -j DNAT --to-destination 192.168.12.100:22
  * ただDDNSだとルーター再起動時に外側IPが変更されるため、その場合は上記のinterface指定する方法を使用
* PREROUTING設定確認方法
  * iptables -L -n -t nat
    * 通常はPREROUTINGのnat tableは表示しないため、iptables -Lでは表示されない
      * 上記のように明示的に指定すると表示される

## 設定永続化方法

* 方法1: 設定永続化のため、iptables-persistentパッケージインストール
  * apt install -y iptables-persistent
    * ダイアログが出るので、現在のルールを保存を選択
    * 今後新規ルール追加時は下記で保存可能
      * iptables-persistent save
  * 設定ファイルは下記に出力される
    * /etc/iptables/rules.v4
    * /etc/iptables/rules.v6
    