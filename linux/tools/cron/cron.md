# cron

## ログ取得方法

* raspberry piの場合
  * 下記コメントアウト解除
    * /etc/rsyslog.conf
      * cron.*        /var/log/cron.log
  * systemctl restart cron
  