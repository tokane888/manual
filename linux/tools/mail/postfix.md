## Postfix

* 参考
  * https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-postfix-on-ubuntu-20-04-ja
* UbuntuのdefaultのMTA(Mail Transfer Agent)
    * 他にexim4もメインリポジトリに入っている
    * MTAはSMTPプロトコルでメッセージをやり取りするのでSMTPサーバとも呼ばれる
    * ubuntu, debianではeximを標準に採用したとのこと
    * CentOSではpostfixを標準に採用
* 動確環境
    * Ubuntu 18.04

### 導入

* sudo権限を付与されたroot以外のuser作成
  * 下記はmail-managerという名前のユーザーを作成する例
    * adduser mail-manager
    * usermod -aG sudo mail-manager
* sudo apt-get install -y postfix
    * 後で設定するので質問されたらデフォルト値を選択
* sudo dpkg-reconfigure postfix
    * (インストール時設定より詳細な設定が可能)
    * "あなたの用途にあったメールサーバ設定形式を選んでください"
      * gitlabなどの通知メールサーバとして使用する場合
        * "インターネットサイト"
    * システムメール名: ~~.mydns.jp
    * Root and postmaster mail recipient: (一旦デフォルトのユーザー名)
        * 上の方で作成したroot以外のuserを指定しておく
    * メールを受け取る他の宛先: ~~.mydns.jp
    * メールキューの同期更新を強制: No
    * ローカルネットワーク: 127.0.0.0/8
    * メールボックスのサイズの制限: 0
    * ローカルアドレス拡張文字: +
    * 利用するインターネットプロトコル: すべて
* 設定調整
  * 後述の調整は基本的にすべて下記に書き込まれるが、直接編集は推奨されない
    * /etc/postfix/main.cf
  * mail boxディレクトリ設定
    * su mail-manager
    * sudo postconf -e 'home_mailbox= Maildir/'
    * mkdir Maildir
  * 任意のメールアカウントをlinuxシステムアカウントにマッピングする設定
    * sudo postconf -e 'virtual_alias_maps= hash:/etc/postfix/virtual'
  * メールアカウントのマッピング設定
    * /etc/postfix/virtual
      * mail-manager@hoge.comでメールを受信し、そのメールをlinuxユーザーのmail-managerに配信したい場合下記のように記載
        ```
        mail-manager@hoge.com mail-manager
        admin@hoge.com mail-manager
        ```
    * 設定反映
      * sudo postmap /etc/postfix/virtual
      * sudo systemctl restart postfix
  * メールクライアントインストールとMaildir構造初期化
    * MAIL環境変数設定
      * echo 'export MAIL=~/Maildir' | sudo tee -a /etc/bash.bashrc | sudo tee -a /etc/profile.d/mail.sh
    * 変数を現在のセッションに読み込み
      * source /etc/profile.d/mail.sh
    * s-nailクライアントをインストール
      * sudo apt install -y s-nail
    * /etc/s-nail.rcの末尾に下記追記
      ```
      set folder=Maildir
      set record=+sent
      ```
      * Maildirディレクトリを内部folder変数に設定
      * 送信済みメールをfolder変数として設定されたディレクトリ内に保存するためsend mboxファイル作成
  * home dirにMaildir構造を作成するため、s-nailで自分にメール送信
    * sentファイルはMaildirが作成されないと利用できない
      * 最初のメールでは書き込みを無効化する必要がある
        * -Snorecordオプションで無効化
    * echo 'init' | s-nail -s 'init' -Snorecord mail-manager
      * /home/mail-manager/Maildir/new/ 配下にメールが保存される
* クライアントのテスト
  * 送信済みのメッセージが受信出来ているか確認
    * 一度terminalを閉じて環境変数再読み込み
    * s-nail
    * メール一覧が表示され、enter押下でメッセージが表示される
    * h入力後enter押下でメッセージ一覧に戻る
    * 読んだメールのstatusがR(Read)になったことが確認できる
    * 当該メッセージは不要なのでd => enter で削除
    * q => enter でターミナルに戻る
  * テキストメッセージ送信テスト
    * 改行を含む適当なメッセージファイルを作成
      * vi ~/test_message
    * 送信
      * cat ~/test_message | s-nail -s 'Test email subject' -r mail-manager@joyhome.mydns.jp (送信先メールアドレス)
    * 

### 設定ファイル

* /etc/postfix/main.cf
* /etc/mailname
    * デフォルトのドメイン名格納してるっぽい
    * 中身1行のみ(~~.mydns.jp)

### 操作

* 送信待ちmail確認
    * mailq

TODO: 追記