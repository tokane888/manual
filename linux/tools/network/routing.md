## routing

### Ubuntu

* routing情報取得
  * ip r
    * ip route でもOK
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