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
    * 