## deb関連のエラー対応

### apt install時エラー

* `N: Unable to package (パッケージ名)`
  * [Ubuntuパッケージ検索](https://packages.ubuntu.com/ja/)で当該パッケージのリポジトリ検索
    * /etc/apt/sources.list に当該リポジトリ追加
    * apt update -y
* apt install (packageA) 実行時に、下記のエラーが出る場合がある
  * `Depends: packageB (version) but it is not going to be installed`
    * apt install (packageA) (packageB) を実行
      * 依存関係ツリーの中に、固定バージョンのパッケージに依存していものがある場合等に発生
        * 連続してやっていき、最後に依存パッケージが全部列挙できる
          * apt list --installed (依存パッケージ一覧) でインストール済みの依存パッケージ列挙可能
          * apt purge -y (依存パッケージ一覧) で強引だけど解決
            * gcc-8-baseとかが依存先の場合消せないので解決できない。
              * TODO: 対策検討
        * 古いパッケージが残っている場合にこの問題が発生するっぽい。色々試した
          * apt upgrade => ✕
          * apt autoremove => ✕
          * apt --fix-broken install => ✕
          * 依存先パッケージapt purge => ○
            * openssh-serverのインストールに失敗する問題は、openssh-clientのuninstallで直った
* `The following packages have unmet dependencies`
  * dpkg -i (package名) でインストールしたパッケージの依存関係に問題があった場合に発生を確認
    * 下記で当該パッケージを削除して対応
      * dpkg -P (package名)
        * 依存関係の問題で上記が実行できない場合もある

### 汎用対応策

* ログ確認
  * cat /var/log/apt/history.log
* aptitude install (パッケージ名)
  * 対応策を提示し、y/nでyを入力で対応策を実施してくれる
    * 1つ目を試してだめな場合、nを押すと2つ目を表示してくれる
  * 注意！）essentialなパッケージをまとめて削除する場合等があるので、-yをつけず、詳細を確認して実行
    * ubuntuのGUIが死んだケースあり