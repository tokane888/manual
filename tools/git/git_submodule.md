# git submodule

* submodule追加
  * git submodule add -b (必要ならブランチ名) https://gitlab.***
* git submoduleのurl変更方法
  * 変更前後のrepositoryに同じcommit IDのcommitがある場合
    * .gitmoduleのurl書き換え
    * git submodule sync
    * git submodule update --init --recursive --remote
    * 参考) https://stackoverflow.com/questions/60003502/git-how-to-change-url-path-of-a-submodule
  * 変更前後のrepositoryに同じcommit IDのcommitがない場合
    * cat .gitmodules
    * git submodule deinit -f (path)
    * git rm (path)
    * git submodule add --force --name (name) (git url) (path)
      * --forceは同じpathに別のsubmoduleを割り当てる場合に必要
    * .gitmodules上でurl変更
    * git add .gitmodules
    * git submodule sync
    * git submodule update --init --recursive --remote