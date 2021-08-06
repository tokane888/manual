## ラズパイネットワーク設定のトラブルシューティング

### SSID関連

* 自分のSSID確認
  * iwgetid
* 周囲のSSID一覧表示
  * iwlist wlan0 scan | grep ESSID

### 設定ファイル

* /etc/wpa_supplicant/wpa_supplicant.conf
  * 複数wifi設定時
    * priority: 高い値の方に優先して接続される
  * 単一IPにした
* /etc/dhcpcd.conf
  * 固定IP設定したけどだめ => ルーター再起動してもだめ
* /etc/network/interfaces は古い設定ファイル
  * 当該ファイルにもコメントがあり、これの使用は非推奨
* wifi起動・停止コマンド
  * ifdown wlan0
  * ifup wlan0
  * unknown interface wlan0出る
* systemctl status wpa_supplicant