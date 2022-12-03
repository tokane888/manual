# xkb

* X11用のキーマップ設定ツール
  * waylandでは使用不可

## 概要

* key code: 物理キーと1対1対応
* keysym: key codeと実際の役割の対応

## コマンド

* key codeとkeysymの対応確認: xmodmap -pke
* 物理キーのkey code確認: xev 
  * キー押下で下記のようにkeycodeとkeysymの対応が出力される
    * state 0x0, keycode 42 (keysym 0x67, g), same_screen YES,
  * surfaceの場合
    * caps lockはkeycode 66, keysym 0xffe5
    * 左ctrlはkeycode 37, keysym 0xffe3
* セッション中のみキー置き換え
  * 例
    * g => h
      * xmodmap -e "keycode 42 = h"
    * caps lock => 左ctrl
      * xmodmap