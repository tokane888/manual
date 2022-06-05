# dovecot

* メール受信サーバ

## 導入

* 参考  
  * https://qiita.com/mizuki_takahashi/items/1b33e1f679359827c17d
* apt install -y dovecot-common dovecot-imapd dovecot-pop3d
* 設定調整
  * /etc/dovecot/dovecot.conf
    * IPv4しか受信しない場合下記記載
      * listen = *
  * /etc/dovecot/conf.d/10-auth.conf
    * plain text認証許可
      * disable_plaintext_auth = no
      * auth_mechanisms = plain login
  * /etc/dovecot/conf.d/10-mail.conf
    * 30行目をMaildir形式に変更
      * mail_location = maildir:~/Maildir
  * /etc/dovecot/conf.d/10-master.conf 
    * smtp認証周りの設定
    * 100行目付近コメントアウトして追記
      ```
      # Postfix smtp-auth
      unix_listener /var/spool/postfix/private/auth {
          mode = 0666
          user = postfix
          group = postfix
      }
      ```
  * /etc/dovecot/conf.d/10-ssl.conf
    * SSL有効化
      * ssl = no
      * 12,13行目の下記コメントアウト
        ```
        #ssl_cert = </etc/dovecot/dovecot.pem
        #ssl_key = </etc/dovecot/private/dovecot.pem
        ```
  * 設定反映
    * systemctl daemon-reload && systemctl restart dovecot
* テスト
  * mail root@localhost
  * 下記にメールが配信されていることを確認
    * /root/Maildir
* 外部からメール受信可能に
  * リンク先参考にルーターの下記ポートを開放
    * https://qiita.com/mizuki_takahashi/items/1b33e1f679359827c17d#-%E3%83%9D%E3%83%BC%E3%83%88%E8%A7%A3%E6%94%BE
    * 25, 465, 587, 110, 995, 143, 993
  * グローバルip取得