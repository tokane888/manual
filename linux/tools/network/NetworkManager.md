# Network Manager

## 設定ファイル

- 下記の順に読まれる。後から読んだ物を優先
  - usr/lib/NetworkManager/conf.d/
  - /run/NetworkManager/conf.d
  - NetworkManager.conf
  - /etc/NetworkManager/conf.d
  - 
- 変更方法
  - 一時的な変更
    - nmcliコマンドで変更
  - 恒久的な変更
    - .confファイルに記載し、OS再起動か下記実行
      - systemctl reload NetworkManager

## 関連ツール構成

- dhcp
  - 内蔵dhcpサーバ使用
- dns
  - dnsmasq又はsystemd-resolvedを使用
    - ubuntuデフォはsystemd-resolved
      - 昔はdnsmasqとの組み合わせはだったようだが、下記手順で設定しても正常動作しない
  - /etc/resolv.confが下記へのシンボリックリンクだとsystemd-resolved使用
    - /run/systemd/resolve/stub-resolv.conf
      - ubuntu 22.04はこれ
    - /run/systemd/resolve/resolv.conf
    - /lib/systemd/resolv.conf
    - /usr/lib/systemd/resolv.conf
  - dnsmasq使用方法
    - apt install -y dnsmasq
    - systemctl disable dnsmasq
      - serviceは使用しないため
    - /etc/NetworkManager/conf.d/dns.conf 作成して下記記載

        ```
        [main]
        dns=dnsmasq
        ```
    - /etc/NetworkManager/dnsmasq.d/ 配下に必要なdnsmasq設定記載
      - 下記は無視される
        - /etc/dnsmasq.conf
        - dnsmasq.service
          - このserviceは使用されない
    - nmcli general reload
      - NetworkManagerが自動的にdnsmasqを起動し、/etc/resolv.confに127.0.0.1を追加
    - 

## nmcliコマンド

- 参考) https://wiki.archlinux.jp/index.php/NetworkManager
- 近くのwifi一覧表示
  - nmcli device wifi list
- wifi接続
  - mcli device wifi connect SSID_または_BSSID password パスワード
- ネットワークデバイスと状態のリスト表示
  - nmcli device
- wifiをoffに
  - nmcli radio wifi off
- 
