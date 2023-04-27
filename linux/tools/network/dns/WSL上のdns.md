# WSL上のdns

* /etc/resolv.conf に下記を記載しておく
  * nameserver 8.8.8.8
* /etc/wsl.confに下記を上書きし、/etc/resolv.confが自動で上書きされることを抑止
  ```
  % cat /etc/wsl.conf
  [boot]
  systemd = true

  [network]
  generateResolvConf = false
  ```
* WSL上でのdocker使用時は下記を/etc/docker/daemon.jsonに追記
  ```
  {
    "dns": ["8.8.8.8"]
  }
  ```
  * 上記記載後、下記実行
    * systemctl restart docker