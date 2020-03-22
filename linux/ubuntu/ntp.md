## ubuntuのntp設定

### systemd-timesyncd

* 概要
    * シンプルなntp client
    * ntpサーバとしての機能はない
* デフォルトの同期先
    * ntp.ubuntu.com
* serviceの状態確認
    * systemctl status systemd-timesyncd
* 同期先変更
    * 下記ファイルの NTP= に同期先記載してコメントアウト解除
        * /etc/systemd/timesyncd.conf
    * サービス再起動
        * systemctl restart systemd-timesyncd

### 類似パッケージ

* ntp
    * 古いntp client/server package
    * デフォルトでは入っていない
* Chrony
    * 新しいntp client/server package
    * デフォルトでは入っていない
