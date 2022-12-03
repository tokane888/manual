# AutoKey

## 検証環境

* ubuntu 22.04

## setup

* 設定ファイルのバックアップを取る
  * /usr/share/X11/xkb/symbols/inet
* 参考) https://qiita.com/Shumei_Ito/items/ce6ae698fcfbd1fd8a42
  * 参考リンク元に設定。キー入力きついので詳細略
* wayland => X11に切り替え
  * 注) GPUが不安定になる可能性もあるため、autokeyを使うべきか微妙
  * 2022/12現在autokeyがwaylandサポートしていないため
  * /etc/gdm3/custom.conf 開く
  * 下記追記
    * WaylandEnable=false
  * 再起動
* caps lock => ctrl置き換え
  * autokey起動
  * Edit => preferences => general => Disable handling of the Capslock key有効化
  * Edit => preferences => general => Automatically start Autokey at login有効化