# android shell

## 関連コマンド

* ルーティングテーブル確認
  * netstat -nr
* dns確認
  * getprop net.dns1
  * getprop net.dns2
* dns設定
  * setprop net.dns1 (IP)

## ip関連挙動

* 動確環境
  * android 11
  * pixel 3a
* テザリング時のnetwork interface割当
  * androidがルーター配下にあるとき
    * wlan0: ルーター側
    * wlan1: テザリングlan
    * rmnet_data2: (使用されていないはずだが、wanから切り替えた場合等は古いipが残る)
  * androidが直接wanに接続しているとき
    * wlan0: (割り当てなし)
    * wlan1: テザリングlan
    * rmnet_data2: wan