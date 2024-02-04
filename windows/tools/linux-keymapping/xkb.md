# xkb

* X11用のキーマップ設定ツール
  * waylandでは使用不可？

## 概要(現状のりかい。全部正しいかは不明)

* 用語
  * ディスプレイマネージャ
    * キーボード、マウス、ディスプレイの入出力を扱う
    * X.org, wayland等
  * xkbはX.orgの1要素
    * キーボードの入出力を扱う
* keycode: 物理キーと1対1対応
* keysym: key codeと実際の役割の対応
* キーボードからの入力のlinuxにおける処理手順
  1. キーボードからscancodeをcpuへ送信
  2. kernelがscancodeをkeycodeに置き換え
     1. このマッピングはudev又はsetkeycodesで編集可能
        1. ただusbキーボードへは通常設定が反映されないなど設定のハードウェア依存が強い
  3. xkbがkeycode => keysymに置き換え
     1. このマッピングはxkbで可能
        1. 設定ファイル直接編集が分かりやすい気がする
           1. /usr/share/X11/xkb/symbols/inet
              1. xkb_symbols項目でkeycode => keysymのマッピング設定
  4. autokeyはkeysym => keysymへの置き換えツールっぽい

## コマンド

* key codeとkeysymの対応確認: xmodmap -pke
* 物理キーのkey code確認: xev 
  * キー押下で下記のようにkeycodeとkeysymの対応が出力される
    * state 0x0, keycode 42 (keysym 0x67, g), same_screen YES,
      * stateは修飾キー
        * 各ビットがそれぞれのキーを表す
          * 下からshift, lock, ctrl, mod1, mod2, ...
  * surfaceの場合
    * caps lockはkeycode 66, keysym 0xffe5
    * 左ctrlはkeycode 37, keysym 0xffe3
* セッション中のみキー置き換え
  * 例
    * g => h
      * xmodmap -e "keycode 42 = h"
    * caps lock => 左ctrl
      * xmodmap
* 修飾キーとmodの対応確認
  * xmodmap
    ```
    # xmodmap
    xmodmap:  up to 4 keys per modifier, (keycodes in parentheses):

    shift       Shift_L (0x32),  Shift_R (0x3e)
    lock        Caps_Lock (0x42)
    control     Control_L (0x25),  Caps_Lock (0x42),  Control_R (0x69)
    mod1        Alt_L (0x40),  Meta_L (0x66),  Meta_L (0xcd)
    mod2        Num_Lock (0x4d)
    mod3      
    mod4        Super_L (0x85),  Super_R (0x86),  Super_L (0xce),  Hyper_L (0xcf)
    mod5        ISO_Level3_Shift (0x5c),  Mode_switch (0xcb)
    ```
* showkey
  * showkey -s
    * 押下されたキーのスキャンコード表示
  * showkey -k
    * 押下されたキーのkeycode表示
* xev
  * 押下したキーのkeysymとstate確認
* keysym一覧
  * /usr/include/X11/keysymdef.h
* 役割が重複するツールが多すぎる
  * xmindのxorgファイルにまとめた
