## git

### 各種操作

* リモート含むブランチ一覧表示
  * git branch -a
* リモートブランチ削除
  * git push origin --delete (ブランチ名)

### ssh関連

* httpsでcloneしたリポジトリを後からsshに変更
    * httpsでcloneしていることの確認方法
        * git remote -v で、originがhttpsになっていればhttpsでcloneしている
        ```
        $ git remote -v
        origin  https://github.com/tokane888/manual.git (fetch)
        origin  https://github.com/tokane888/manual.git (push)
        ```
    * sshに変更する方法
        * git remote set-url origin で変更
        ```  
        $ git remote set-url origin git@github.com:tokane888/manual.git
        ```
    * 変更後
        * git remote -v でoriginが git@~~ にっていることを確認
        ```
        $ git remote -v
        origin  git@github.com:tokane888/manual.git (fetch)
        origin  git@github.com:tokane888/manual.git (push)
        ```
* git clone 時に使用する秘密鍵を特定ドメインでのみ変更
  * 下記のようにIdentityFile指定を.ssh/configに追加
    ```
    Host github.com
    User git
    IdentityFile ~/.ssh/git_id_rsa
    ```