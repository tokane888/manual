# ICMP

* 第4層のプロトコル
  * ポートはTCPの概念なので、ICMPでは関係しない
* ping等
* wiresharkで"icmp"で抽出可能

## ヘッダ

* 参考) https://beginners-network.com/icmp.html
* Type
  * 0: echo reply(pingへの応答)
  * 3: destination unreachable
  * 5: redirect
  * 8: echo request(pingへの応答依頼)
  * 11: time exceeded(許容可能なルーターの数を超えたため破棄)
* Type=3(destination unreachable)のときのCode一覧
  * 0: network unreachable
  * 1: host unreachable
    * サーバーがダウンしてarp解決出来ない等
    * ラズパイルーター作成時に発生した
  * 3: port unreachable
  * 4: fragmentation needed and DF set	
  * 6: destination network unknown
  * 13: communication administratively prohibited by filtering
