# fdisk

* パーティションテーブル表示
  * fdisk -l /dev/sda
    * fdisk /dev/sda => p でも可能
    * 複数HDDの場合は/dev/sdb などのように末尾のアルファベットを変更
    * 出力項目
      * Disklabel type
        * MBRのtype
      * Disk identifier
        * MBR上に生成されたランダムな識別子
    * 古いコマンドで、GPT非対応
      * GPTのHDDに対して実行すると、互換性のために残されているMBRを参照する
        * 基本的にgdiskなどを使用するのが望ましい
      * GPT非対応のはずだが、manを見るとGPT関連情報がある。。
  * 既存のパーティションのサイズの縮小は無理っぽい
  * 既存パーティションを削除してサイズ変更は下記参照
    * https://access.redhat.com/ja/articles/2211741

