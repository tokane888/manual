# xkb

* X11用のキーマップ設定ツール
  * waylandでは使用不可？

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
* hyperキーがsuper(win)と同じ動きをする問題の対応
  * /etc/rc.localに下記を実行
    * setxkbmap -print | sed -e '/xkb_symbols/s/"[[:space:]]/+local&/' | xkbcomp -I${HOME}/.config/xkb - ${DISPLAY}
    * TODO: 微妙に不安定なので調査
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
    