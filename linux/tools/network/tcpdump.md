## tcpdump

* tcpdumpで通信を出力
  * 10:09:50.352259 IP tom-PC-LZ550NSB.ssh > 192.168.11.3.50219: Flags [P.], seq 11667036:11667200, ack 6837, win 501, length 164
  * 時刻 IP 送信元host名.port > 送信先.port: Flags [フラグ], seq (seq1):(seq2), ack ack番号, win windowサイズ, length 最大セグメントサイズ
      * 送信元のportはプロトコル名で表示される
          * ssh => 22
          * 対応は /etc/services 参照
      