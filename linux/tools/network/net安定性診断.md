# net安定性診断

* 特定プロセスのudpパケットロス割合を確認する場合
  * cat /proc/(process ID)/net/udp
    * 出力例

      ```
      # cat /proc/28580/net/udp
       sl  local_address rem_address   st tx_queue rx_queue tr tm->when retrnsmt   uid  timeout inode ref pointer drops
       271: 00000000:14E9 00000000:0000 07 00000000:00000000 00:00000000 00000000   115        0 28241 2 ffff9db13f14de80 0
      1115: 3500007F:0035 00000000:0000 07 00000000:00000000 00:00000000 00000000   101        0 19406 2 ffff9db211c77080 0
      1130: 00000000:0044 00000000:0000 07 00000000:00000000 00:00000000 00000000     0        0 31922 2 ffff9db13ef8cc80 0
      1342: 00000000:2118 00000000:0000 07 00000000:00000000 00:00000000 00000000     0        0 39796 2 ffff9db18935da00 0
      1399: 00000000:E151 00000000:0000 07 00000000:00000000 00:00000000 00000000   115        0 28243 2 ffff9db13f14da00 0
      1693: 00000000:0277 00000000:0000 07 00000000:00000000 00:00000000 00000000     0        0 80421 2 ffff9db215a3e300 0
      1964: 00000000:9386 00000000:0000 07 00000000:00000000 00:00000000 00000000  1000        0 202014 2 ffff9db107259f80 0
      1965: 00000000:9387 00000000:0000 07 00000000:00000000 00:00000000 00000000  1000        0 202015 2 ffff9db107258000 0
      ```
    * 各カラムの意味
      * 参考) https://stackoverflow.com/questions/23753306/meaning-of-fields-in-proc-net-udp/23755250
      * sl: kernelのhash slot？とのこと
      * local_address: local addressとport番号のペア
      * rem_address: remote addressとport番号のペア
      * st: socketの内部status
      * tx_queue: 送信待ちdata queueによるメモリ使用量
      * rx_queue: 受信待ちdata queueによるメモリ使用量
      * uid: socketを作成したなにかのuid

## 映像受信時のノイズ原因分析

* 注意事項
  * vlc media playerで映像を視聴した場合、vlcのプロセス自体は映像が乱れてもpacket lostしない
