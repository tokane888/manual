# gdisk

* パーティションテーブル表示
  * gdisk -l /dev/sda
    * gdisk /dev/sda => p でも可能
    * 複数HDDの場合は/dev/sdb などのように末尾のアルファベットを変更
    * GPT対応
* GUIDパーティションテーブル(GPT)表示
  * gdisk /dev/sda => b => (出力先ファイル名指定)
  * hexdump -C (出力先ファイル名)