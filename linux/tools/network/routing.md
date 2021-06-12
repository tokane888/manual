## routing

### Ubuntu

* routing情報取得
  * ip r
    * ip route でもOK
    * スクリプトでの処理に向いている
      * 例
        ```
        tom@pi4:~$ ip r
        default via 192.168.11.1 dev wlan0 proto dhcp src 192.168.11.103 metric 303
        192.168.11.0/24 dev wlan0 proto dhcp scope link src 192.168.11.103 metric 303
        ```
  * route -n
    * こちらの方が見やすい
      * 例
        ```
        tom@pi4:~$ route -n
        カーネルIP経路テーブル
        受信先サイト    ゲートウェイ    ネットマスク   フラグ Metric Ref 使用数 インタフェース
        0.0.0.0         192.168.11.1    0.0.0.0         UG    303    0        0 wlan0
        192.168.11.0    0.0.0.0         255.255.255.0   U     303    0        0 wlan0
        ```
      * ただこちらは古く、今後ipへ移行の流れ
* マニュアルでip routeコマンドの詳細確認可能
  * man ip route
* ip routeの出力の見方
  * via: ネクストホップのルーター
  * dev: 対象デバイス
  * proto kernel: kernelが自動生成した経路
  * proto dhcp: dhcpが自動生成した経路
* ルート選択方法
  * 上から順に検索
    * 最も一致するnetmaskが長いroute選択
    * 一致するnetmaskが同じrouteが複数ある場合、metricが小さいroute選択
    * metricが同じrouteが複数ある場合、ip tableの最後のrouteを選択
* 一時的なルーティングルール削除例
  ```
  # route -n
  カーネルIP経路テーブル
  受信先サイト    ゲートウェイ    ネットマスク   フラグ Metric Ref 使用数 インタフェース
  0.0.0.0         172.168.11.1    0.0.0.0         UG    202    0        0 eth0
  0.0.0.0         192.168.11.1    0.0.0.0         UG    303    0        0 wlan0
  172.168.11.0    0.0.0.0         255.255.255.0   U     202    0        0 eth0
  192.168.11.0    0.0.0.0         255.255.255.0   U     303    0        0 wlan0
  # route del -net 0.0.0.0 netmask 0.0.0.0 gw 172.168.11.1
  # route -n
  カーネルIP経路テーブル
  受信先サイト    ゲートウェイ    ネットマスク   フラグ Metric Ref 使用数 インタフェース
  0.0.0.0         192.168.11.1    0.0.0.0         UG    303    0        0 wlan0
  172.168.11.0    0.0.0.0         255.255.255.0   U     202    0        0 eth0
  192.168.11.0    0.0.0.0         255.255.255.0   U     303    0        0 wlan0
  ```
  * OS再起動で元に戻る。検証用