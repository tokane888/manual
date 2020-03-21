## ubuntuのntp設定

* デフォルトで同期有効
    * ntp.ubuntu.com
* ntp serviceの状態確認
    * systemctl status systemd-timesyncd
* 同期先変更
    * 下記ファイルの NTP= に同期先記載してコメントアウト解除
        * /etc/systemd/timesyncd.conf
    * サービス再起動
        * systemctl restart systemd-timesyncd