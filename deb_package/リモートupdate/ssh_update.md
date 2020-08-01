# 接続先のsshパッケージを、ssh接続して更新

* これは成功するっぽい

## debパッケージ内容

* 下記で取得
  * apt download openssh-client
  * dpkg -e *.deb
* pre, post関連ファイル一覧
  * postinst
  * postrm
  * prerm
* postinst
  * action=configureの場合のみ処理が行われる
    * action=$1
      * install, update時
  * create_alternatives()
    * 各種r*ツールの代替品を生成
      * userが変更した可能性のある既存の代替品は変更しない
      * 最後にcleanupするとかなんとか
    * update-alternatives
      * sshの代替指定を削除しているっぽい
* prerm
  * TODO: $1の値確認
* postrm
  * purge時以外は処理なし

## TODO

* 実際にssh接続してupdate
  1. 接続先PCに古いopenssh-clientパッケージインストール
  2. sshで接続先PCに接続し、openssh-client update
  3. update成功することを確認
 