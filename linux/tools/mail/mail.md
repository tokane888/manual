## mail

* (参考) https://help.ubuntu.com/community/MailServer

### mail送信処理概要

* MUA(Mail User Agent)
    * mail作成
    * MTAへmail送信
* MTA(Mail Transfer Agent)
    * mail addressから配送先メールサーバ検索
    * mailを別のMTAへ送信
        * 25番portから送信
            * 迷惑mail対策でOP25B(Outbount Port 25 Block)されることがある
    * mailを受け取ったMTAは、ローカル配送プログラムであるMDAへmailを渡す
* MDA(Mail Delivery Agent)
    * 宛先のmail boxへmailを格納
* 受取人
    * POPサーバ、IMAPサーバ経由で自分のmail boxからmailを取り出す

### mail filtering機能

* 代表的な物
    * PostfixAmavisNew
    * PostfixGreyListing
    * EximClamAV

### MDA(Mail Delivery Agent)

* IMAP, POP3サーバのこと
* 代表的な物
    * Dovecot
    * Courier
    * MailServerCourierSpamAssasin
    * Cyrus

### WebMail

* 代表的な物
    * Squirrelmail
    * OpenWebMail

### Mailling lists

* 代表的な物
    * Mailman