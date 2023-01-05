# hwdb

* キーボードの物理キーとkeycodeの対応設定
* wayland前提。X.orgでは機能しない
* 参考
  * https://endaaman.me/tips/wayland
  * https://wiki.archlinux.jp/index.php/%E3%82%B9%E3%82%AD%E3%83%A3%E3%83%B3%E3%82%B3%E3%83%BC%E3%83%89%E3%82%92%E3%82%AD%E3%83%BC%E3%82%B3%E3%83%BC%E3%83%89%E3%81%AB%E3%83%9E%E3%83%83%E3%83%97

## 概要

* 関連設定ファイル
  * デフォルトのマッピングファイル
    * /usr/lib/udev/hwdb.d/60-keyboard.hwdb
  * オプションの設定ファイル置き場所
    * /etc/udev/hwdb.d/*.hwdb
* 詳細な設定方法は下記サイト参照
  * https://endaaman.me/tips/wayland
* surfaceでは下記設定でcaps lock => ctrlに置き換え成功
  * /etc/udev/hwdb.d/01-surface3.hwdb
    ```
    evdev:input:*
      KEYBOARD_KEY_70039=leftctrl
    ```
* 下記実行で設定反映
  * udevadm hwdb --update
    * 再起動しても上記を実行していないと反映されない
    * 再起動の度に上記を実行する必要はない