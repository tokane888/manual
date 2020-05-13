# File System Hierarchy Standard

* Linuxに於けるディレクトリ構造の標準

## よく使うディレクトリ

* /bin
  * システム標準の実行ファイルの配置場所
  * Priority: required のパッケージの実行ファイル。下記で確認可能
    * apt show (パッケージ名)
* /usr/bin
  * 追加でinstallされるパッケージの実行ファイル配置場所
  * Priority: extra, optional等のパッケージの実行ファイル
* /usr/sbin
  * 追加でinstallされるパッケージの、管理者権限が必要な実行ファイル配置場所
  * Priority: extra, optional等のパッケージの実行ファイル