## 概要

* sshでクライアントOSからlinuxへ、sshでパスワード入力無しで接続可能にするまでの手順
* サーバ側手順含む

### クライアント側手順

* `cd ~/.ssh`
* 上記ディレクトリにパスなしのid_rsa.pubが有る場合
    * TODO: パスの有無の判定方法確認
        * パスありだと、ログイン時にパス入力が必要
    * サーバ側へ公開鍵連携
        * `ssh-copy-id -i id_rsa.pub `[サーバ側user名]@[サーバ側IPorドメイン]`
* 上記ディレクトリにパスなしのid_rsa.pubが無い場合
    * 公開鍵／秘密鍵ペア生成
        * `ssh-keygen -N '' -f id_rsa.pub -t rsa -b 4096`
            * -fでのファイル名指定はなくてもOK
    * サーバ側へ公開鍵連携
        * `ssh-copy-id -i id_rsa.pub `[サーバ側user名]@[サーバ側IPorドメイン]`

### サーバ側手順

* ubuntuの場合
    * openssh-server未インストールの場合導入
        * `dpkg -s openssh-server || sudo apt install openssh-server`
