# tcpdump

## 導入

* apt install -y tcpdump

## コマンド等

* tcpdumpで通信を出力
  * 10:09:50.352259 IP tom-PC-LZ550NSB.ssh > 192.168.11.3.50219: Flags [P.], seq 11667036:11667200, ack 6837, win 501, length 164
  * 時刻 IP 送信元host名.port > 送信先.port: Flags [フラグ], seq (seq1):(seq2), ack ack番号, win windowサイズ, length 最大セグメントサイズ
      * 送信元のportはプロトコル名で表示される
          * ssh => 22
          * 対応は /etc/services 参照
* dnsで名前解決先のサーバを確認する方法
  * tcpdump -i tun0 -n -vv dst port 53
    * -i: ネットワークインターフェース名
    * -n: ドメイン名とIPの変換を行わない
    * -vv: 詳細ログ出力
    * dst port: 宛先port指定
* 自身の80番portへのリクエストを取得
  * tcpdump -i wlan0 -Q in -n -vv dst port 80
    ```
    192.168.12.100.33024 > 192.168.12.101.80: Flags [P.], cksum 0xe0d7 (correct), seq 0:191, ack 1, win 491, options [nop,nop,TS val 3576595338 ecr 3328893325], length 191: HTTP, length: 191
    POST /lte-button/click HTTP/1.1
    Host: joyhome.mydns.jp:50080
    User-Agent: curl/7.68.0
    Accept: */*
    Content-Type: application/json
    Content-Length: 36

    {"serial": "test", "type": "single"}[!http]
    ```
