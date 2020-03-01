## CentOSネットワーク設定

### wifi接続(手順作成中。。)

* デバイス一覧確認
  * nmcli -o
* 上記で"Plugin missing"と表示された場合
  * windowsなどで、下記から最新のプラグインダウンロード
    * https://pkgs.org/download/NetworkManager-wifi
  * 上記で取得したファイルをUSBメモリでCentOSへ移動
    * TODO: 依存関係が解決出来ずインストール失敗。有線でつなぐ等して手順確立

### ポート開放

* ポート開放
  * 例) 8001番ポートを開放
    * firewall-cmd --zone=public --add-port=8001/tcp --permanent 
* firewall設定リロード
  * firewall-cmd --reload
* (firewall設定確認)
  * firewall-cmd --list-all