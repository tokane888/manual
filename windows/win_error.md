## windows関連エラー対応

* タスクバーが勝手に閉じなくなる問題
  * タスクマネージャを開いて"Windows Explorer"を右クリック
  * "restart"押下
* エクスプローラー等への、最近開いたファイルの表示／非表示切り替え
  * デスクトップ右クリック
  * "Personalize"
  * 左メニューから"Start"
  * "Show recently opened item in ~~"
    * on/off切り替え
  * 注意）エクスプローラーからfolder option開いて設定するのは今は不可能
    * 以下再現
      * エクスプローラー右上のvアイコン押下
      * "View"タブ
      * "Options"
      * "General"タブ
      * "Show recently used files in Quick access"
        * チェックはできるが、OK押下後、再度見に来るともとに戻っている