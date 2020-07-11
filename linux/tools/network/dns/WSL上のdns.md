# WSL上のdns

* 名前解決に失敗する場合があった
  * /etc/resolv.conf に下記を記載しておく
    * nameserver 8.8.8.8
  * WSLは特殊な環境で、systemdが使用不可能なため、詳細な原因を追い辛い