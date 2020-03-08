## 概要

* sshでクライアントOSからlinuxへ、sshでパスワード入力無しで接続可能にするまでの手順
* サーバ側手順含む
* rootでのssh可能にする手順も記載

### クライアント側手順

* `cd ~/.ssh`
* 上記ディレクトリにパスなしのid_rsa.pubが有る場合
    * TODO: パスの有無の判定方法確認
        * パスありだと、ログイン時にパス入力が必要
    * (clientがlinux系の場合)サーバ側へ公開鍵連携
        * `ssh-copy-id -i id_rsa.pub `[サーバ側user名]@[サーバ側IPorドメイン]`

* 上記ディレクトリにパスなしのid_rsa.pubが無い場合
    * 公開鍵／秘密鍵ペア生成
        * `ssh-keygen -N '' -f id_rsa.pub -t rsa -b 4096`
            * -fでのファイル名指定はなくてもOK
    * (clientがlinux系の場合)サーバ側へ公開鍵連携
        * `ssh-copy-id -i id_rsa.pub `[サーバ側user名]@[サーバ側IPorドメイン]`
* クライアントがwindowsの場合
    * 公開鍵をクリップボードへコピー
        * `cat ~/.ssh/id_rsa.pub | clip`

### サーバ側手順

* ubuntuの場合
    * openssh-server未インストールの場合導入
        * `dpkg -s openssh-server || sudo apt install openssh-server`

* クライアントがwindowsの場合
    * 公開鍵登録
        ```
        vim ~/.ssh/authorized_keys
        (公開鍵をクリップボードから貼り付ける)
        (vim終了)
        chmod 600 ~/.ssh/authorized_keys
        chmod 700 ~/.ssh
        ```

### クライアント側でssh接続先alias設定

* vim ~/.ssh/config
    * 設定を書き込み

        ````
        Host hubu
        HostName hogehoge.mydns.jp
        User tom
        ServerAliveInterval 60  # 60秒ごとに接続通知を行い、勝手に切断されることを抑止
        ```
* chmod 600 ~/.ssh/config
* "ssh hubu" で接続可能に

### rootでssh接続可能に設定

* サーバ側手順
    * sudo su
    * passwd root
    * vim /etc/ssh/sshd_config
        * "PermitRootLogin"で検索し、下記のように書き換え
            * PermitRootLogin yes
    * systemctl restart ssh
* クライアント側手順
    * 一般ユーザーの場合と同様に公開鍵をサーバ側の下記パスへコピー
        * /root/.ssh/authorized_keys
    * chmod 600 /root/.ssh/authorized_keys
