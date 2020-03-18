## OpenVPN

WindowsからUbuntu18.04にOpenVPNで接続する手順

### 導入

#### Windows

* 公式のインストーラー使用
  * https://www.openvpn.jp/download/

### routring方式での接続

#### Ubuntu側(server)

* sudo apt install -y openvpn
* 共通鍵生成
  * openvpn --genkey --secret static.key
* 設定ファイル作成
  * statickey_server.ovpn
    ```
    proto udp
    dev tun
    port 1194
    ifconfig 10.8.0.1 10.8.0.2
    secret static.key
    comp-lzo
    keepalive 10 60
    ping-timer-rem
    persist-tun
    persist-key
    ```
* サーバ起動
  * 

#### WSL側(client)

* sudo apt install -y openvpn
* 共通鍵をサーバ側からコピー
* 共通鍵と同じフォルダに設定ファイル作成
  * statickey_client.ovpn
    ```
    remote <サーバーの実IPアドレス>
    proto udp
    dev tun
    port 1194
    ifconfig 10.8.0.2 10.8.0.1
    secret static.key
    comp-lzo
    keepalive 10 60
    ping-timer-rem
    persist-tun
    persist-key
    ```
TODO: 接続方法追記
* サーバへpingを打って動確
  * ping 10.8.0.1