## git

### 各種操作

* リモート含むブランチ一覧表示
  * git branch -a
* リモートブランチ削除
  * git push origin --delete (ブランチ名)
* git clean
  * clean実行時に消されるファイル一覧取得
    * git clean -n
  * clean実行
    * git clean -f
  * ディレクトリ削除も含めたclean実行
    * git clean -df
