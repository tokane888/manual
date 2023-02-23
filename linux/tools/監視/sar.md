# sar

## 導入

* apt install -y sysstat
* 監視開始
  * systemctl enable --now sysstat
    * statusはactive(exit)になり、10分に1回cronから実行される
* 必要に応じて下記ファイルで監視間隔調整
  * /etc/cron.d/sysstat