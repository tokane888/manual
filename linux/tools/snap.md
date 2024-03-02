## snap

* linuxのディストリビューション非依存のパッケージ管理ソフト
* 依存パッケージも含めてパッケージ化されており、独立性が高いとか
  * 依存問題に時間取られないのはありがたい

### コマンド

* 利用可能なパッケージバージョン一覧取得
  * snap info (パッケージ名)
* パッケージインストール
  * snap install (パッケージ名)
    * sandboxの外に影響があるものは、警告が出るので --classic 付ける
* インストール済みのパッケージ一覧表示
  * snap list
* パッケージ完全削除
  * sudo snap remove (package名) --purge
* パッケージに含まれるファイル一覧表示
  * 注) コマンド1発では無理っぽい
  * cd /var/lib/snapd/snaps
  * ls
    * それっぽいパッケージ.snapを探す
  * sudo unsquashfs -l xmind_29.snap
