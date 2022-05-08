## Postfix

* UbuntuのdefaultのMTA(Mail Transfer Agent)
    * 他にexim4もメインリポジトリに入っている
    * MTAはSMTPプロトコルでメッセージをやり取りするのでSMTPサーバとも呼ばれる
    * ubuntu, debianではeximを標準に採用したとのこと
    * CentOSではpostfixを標準に採用
* 動確環境
    * Ubuntu 18.04

### 導入

* sudo apt-get install -y postfix
    * 後で設定するので質問されたらデフォルト値を選択
* sudo dpkg-reconfigure postfix
    * (インストール時設定より詳細な設定が可能)
    * "インターネットサイト"
    * システムメール名: ~~.mydns.jp
    * Root and postmaster mail recipient: (一旦デフォルトのユーザー名)
        * TODO: 使われ方詳細分かったら追記
        * エラー通知等の受信者？
    * メールを受け取る他の宛先: ~~.mydns.jp
    * メールキューの同期更新を強制: No
    * ローカルネットワーク: 127.0.0.0/8
    * メールボックスのサイズの制限: 0
    * ローカルアドレス拡張文字: +
    * 利用するインターネットプロトコル: すべて

### 設定ファイル

* /etc/postfix/main.cf
* /etc/mailname
    * デフォルトのドメイン名格納してるっぽい
    * 中身1行のみ(~~.mydns.jp)

### 操作

* 送信待ちmail確認
    * mailq

TODO: 追記