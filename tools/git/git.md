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
* git tag
  * リモートのタグをpull
    * git pull --tag
      * git pullだけではtagは同期されない
  * リモートへタグをpush
    * git push origin --tags
* masterブランチと特定のブランチのカレントディレクトリを比較
  * git diff other_branch..master .
* masterブランチから特定ファイル取得
  * git ch master -- test.sh

### トラブルシューティング

* ブランチ切るのを忘れてコミットした場合
  * 旧コミットからブランチ作成
    * git checkout -b <new_branch> <commit_hash>