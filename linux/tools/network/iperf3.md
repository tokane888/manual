## iperf3

### 導入

* server, client側双方に下記でiperf3インストール
  * debian系(ubuntu, raspbian OS等)の場合
    * apt install -y iperf3
* ポート開放
  * 開いていなければ5201番port開放

### 使用方法

* tcpによる計測
  * サーバ側
    * iperf3 -s
  * クライアント側
    * iperf3 -c (サーバ側IP)
* 出力例

  ```
  # iperf3 -c 192.168.11.102
  Connecting to host 192.168.11.102, port 5201
  [  5] local 192.168.11.103 port 41240 connected to 192.168.11.102 port 5201
  [ ID] Interval           Transfer     Bitrate         Retr  Cwnd
  [  5]   0.00-1.00   sec  2.71 MBytes  22.8 Mbits/sec   30    315 KBytes
  [  5]   1.00-2.00   sec  1.68 MBytes  14.1 Mbits/sec   18    171 KBytes
  [  5]   2.00-3.00   sec  2.55 MBytes  21.4 Mbits/sec    0    187 KBytes
  [  5]   3.00-4.00   sec  2.61 MBytes  21.9 Mbits/sec    7    148 KBytes
  [  5]   4.00-5.00   sec  2.98 MBytes  25.0 Mbits/sec    0    158 KBytes
  [  5]   5.00-6.00   sec  2.55 MBytes  21.4 Mbits/sec   20    129 KBytes
  [  5]   6.00-7.00   sec  3.11 MBytes  26.1 Mbits/sec    7    106 KBytes
  [  5]   7.00-8.00   sec  2.98 MBytes  25.0 Mbits/sec    0    126 KBytes
  [  5]   8.00-9.00   sec  3.54 MBytes  29.7 Mbits/sec    1    105 KBytes
  [  5]   9.00-10.00  sec  3.04 MBytes  25.5 Mbits/sec    2   94.7 KBytes
  - - - - - - - - - - - - - - - - - - - - - - - - -
  [ ID] Interval           Transfer     Bitrate         Retr
  [  5]   0.00-10.00  sec  27.8 MBytes  23.3 Mbits/sec   85             sender
  [  5]   0.00-10.00  sec  26.8 MBytes  22.5 Mbits/sec                  receiver

  iperf Done.
  ```
