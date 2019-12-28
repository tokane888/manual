## rpmパッケージの情報取得

### ファイル一覧取得

* リポジトリから
    * `repoquery --list (パッケージ名)`
* インストール済みパッケージから
    * `rpm -ql (パッケージ名)`
* .rpmファイルから
    * `rpm -qlp *.rpm`

### その他

* 特定ファイルを含むパッケージ取得
    * `rpm -qf (ファイルパス)`
        * ex) `rpm -qf /usr/bin/mv`
* リポジトリからパッケージのメタデータ取得
    * `yum info (パッケージ名)`
* rpmファイル取得
    * `yumdownloader (パッケージ名)`
* ソースパッケージ取得
    * `yumdownloader --source (パッケージ名)`
* インストール前後に実行されるスクリプト取得
    * `rpm -qp --scripts (rpmファイル名)`
