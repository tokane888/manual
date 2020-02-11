## git-bash

* git status等での文字化け対策
  * 下記実行
    * git config --global core.quotepath false
* docker exec時にwinpty入力不要に
    * vim /etc/profile.d/aliases.sh
    * 当該箇所に下記のようにdocker追加
    ```
    - for name in node ipython php php5 psql python2.7
    - for name in node ipython php php5 psql python2.7 docker
    ```
    * git-bash再起動