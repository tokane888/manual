# root化関連ツール

* MagiskとSuperSU
  * Magiskの方が新しい
  * SuperSU
    * システムファイルを書き換えてroot取得
      * android 6でセキュリティが強化されるまではこの方法で問題なかった
      * android 6以降ではシステムファイル書き換えが検知され、root化したことを検知されるようになった
  * Magisk
    * systemlessにrootを取得
    * system fileを編集しない
    * boot partitionだけ書き換える
    * 仮想化をうまく使って対応しているとのこと
  
## ツール使用方法

* OS起動時にコマンド実行(Magisk使用)
  * 参考) https://github.com/topjohnwu/Magisk/blob/master/docs/guides.md#boot-scripts
  * 下記パス配下に、実行可能形式のスクリプト配置
    * /data/adb/service.d

## Magisk

* 参考) https://topjohnwu.github.io/Magisk/guides.html
* busybox同梱
  * /data/adb/magisk/busybox
* OS起動時に下記ディレクトリの実行可能なファイル実行
  * /data/adb/service.d
* 