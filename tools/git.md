## git

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