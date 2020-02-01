## deb関連のエラー対応

### インストールエラー

* `N: Unable to package (パッケージ名)`
  * [Ubuntuパッケージ検索](https://packages.ubuntu.com/ja/)で当該パッケージのリポジトリ検索
    * /etc/apt/sources.list に当該リポジトリ追加
    * apt update -y
* apt install (packageA) 実行時に、下記のエラーが出る場合がある
  * `Depends: packageB (version) but it is not going to be installed`
    * TODO: 対応方法確立
    * apt install (packageA) (packageB) を実行
      * 依存関係ツリーの中に、固定バージョンのパッケージに依存していものがある場合等に発生
        * 古いパッケージが残っている場合にこの問題が発生するっぽい。色々試した
          * apt upgrade => ✕
          * apt autoremove => ✕
          * apt --fix-broken install => ✕