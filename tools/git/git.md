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
* merge済み、又はremoteで削除済み(upstream gone)のブランチを削除
  * git fetch -p && for branch in $(git branch -vv | grep ': gone]' | awk '{print $1}'); do git branch -D $branch; done
* localで作成した、remoteに存在しないbranchをremoteに作成してpush
  * git checkout (当該ブランチ)
  * git push origin -u HEAD
* branch名変更
  * git branch -m main master
* gitの全branchを含めてclone
  * for remote in `git branch -r`; do git branch --track ${remote#origin/} $remote; done
  * git fetch --all
  * git pull --all
* remote branch名変更
  * git branch -m test/before test/after
  * git push origin test/after
  * git push origin :test/before
* git push先がhttpsかsshか確認
  * git remote -v
* git push先のhttps/ssh切り替え
  * git remote set-url origin (url)
* 特定のメンバーが1日に追加／削除したソース行数取得
  * git log --author=<メンバー名> --since="1 day ago" --until="1 day ago" --oneline --shortstat
* カレントブランチのlogからログ文字列を含むコミット抽出
   * git log -S "ログ文字列"
* 全ブランチのlogからログ文字列を含むコミット抽出
   * git log -S "ログ文字列" --all
* マージ済みブランチ削除
  * git branch --merged|egrep -v '\*|develop|master|main'|xargs git branch -d

### トラブルシューティング

* ブランチ切るのを忘れてコミットした場合
  * 旧コミットからブランチ作成
    * git checkout -b <new_branch> <commit_hash>
