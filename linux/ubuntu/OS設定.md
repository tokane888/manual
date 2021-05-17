# OS設定

* ubuntu向け設定コマンドなど
* hostnameを永続的に変更
  * hostnamectl set-hostname (new hostname)
  * OS再起動してsudo su実行
  * 下記の警告が表示されるか確認
    * sudo: unable to resolve host (hostname): Name or service not known
      * 例) sudo: unable to resolve host router: Name or service not known
    * 表示される場合は/etc/hostsに下記追記
      * 127.0.0.1 (hostname)
        * 例) 127.0.0.1 router