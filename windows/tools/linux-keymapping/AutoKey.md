# AutoKey

## 検証環境

* ubuntu 22.04

## setup

* 設定ファイルのバックアップを取る
  * /usr/share/X11/xkb/symbols/inet
  * 上記設定ファイルの変換、無変換キー関連箇所修正
    ```
        key <HENK>   {      [ Hyper_L               ]       };
        key <MUHE>   {      [ Meta_L                ]       };
    ```
* 参考) https://qiita.com/Shumei_Ito/items/ce6ae698fcfbd1fd8a42
  * 参考リンク元に設定。キー入力きついので詳細略
* wayland => X11に切り替え
  * 注) GPUが不安定になる可能性もあるため、autokeyを使うべきか微妙
  * 2022/12現在autokeyがwaylandサポートしていないため
  * /etc/gdm3/custom.conf 開く
  * 下記追記
    * WaylandEnable=false
  * 再起動
* 自動起動有効化
  * autokey起動
  * Edit => preferences => general => Automatically start Autokey at login有効化
* pip3で導入したパッケージ使用可能に
  * Edit => preferences => Script Engine => `/usr/local/lib/python3.10/dist-packages`辺りを指定