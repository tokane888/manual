# ble基本操作

## 開発環境構築

* コンソールから接続可能に
  * TODO: 詳細追記
  * switchBot.md記載の手順でgitttoolは使用可能になっていた
* interactiveモードで接続
  * 接続先指定
    * gatttool -I -b (mac address)
      * 例) gatttool -I -b xx:xx:xx:xx:xx:xx
  * 接続
    * connect
* bluetoothがonになっているか確認
  * hciconfig
    * "UP RUNNING"であればOK
    