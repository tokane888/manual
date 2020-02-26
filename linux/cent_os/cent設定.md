## CentOS各種設定

* ポート開放
  * 例) 8001番ポートを開放
    * firewall-cmd --zone=public --add-port=8001/tcp --permanent 
* firewall設定リロード
  * firewall-cmd --reload
* (firewall設定確認)
  * firewall-cmd --list-all