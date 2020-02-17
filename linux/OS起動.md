## OS起動

起動順序
* BIOS/UEFI => ブートローダー => カーネル => init/systemd

### BIOS/UEFI

* BIOS
  * マザーボードの不揮発性メモリに組み込まれているプログラム
  * 決められた優先順位に従ってブートメディアを指定
  * ブートメディアの先頭セクタにあるブートローダーをロード
  * 16bitモードでしか動作しない => 1MBのメモリ空間にしかアクセスできない
    * システムの機能強化が困難
    * マザーボード毎にフォームウェアをアセンブラで開発
* UEFI (Unified Extensible Firmware Interface)
  * BIOSの後継
  * 起動／再開時間短縮
  * 2TB以上の大容量ドライブ使用可能
  * OS読み込み前に信頼されていないコードを実行不可にするセキュアブート対応
  * 旧BIOSとの互換性のために残されているCSM(Compatibility Support Module)は削除の方向

### パーティション

* MBR: Master Boot Record
  * 古い。容量2TB制限
* GPT: GUID partition table
  * 2TB以上OK
