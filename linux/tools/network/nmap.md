# nmap

## コマンド

* 特定subnet上で22番portが空いているデバイスを検索
  * nmap -p 22 192.168.11.255/24
    * ラズパイにsshがつながらない際等に検索可能
    * 例
      ```
      # nmap -p 22 192.168.11.255/24
      Starting Nmap 7.80 ( https://nmap.org ) at 2021-05-31 20:16 JST
      Nmap scan report for 192.168.11.1
      Host is up (0.0052s latency).

      PORT   STATE  SERVICE
      22/tcp closed ssh

      Nmap scan report for test (192.168.11.102)
      Host is up (0.070s latency).

      PORT   STATE SERVICE
      22/tcp open  ssh

      Nmap scan report for pi4 (192.168.11.103)
      Host is up (0.048s latency).

      PORT   STATE SERVICE
      22/tcp open  ssh

      Nmap scan report for kali (192.168.11.104)
      Host is up (0.013s latency).

      PORT   STATE SERVICE
      22/tcp open  ssh

      Nmap done: 256 IP addresses (4 hosts up) scanned in 5.23 seconds
      ```
* 特定subnetの各IPからpingを返すものを取得
  * nmap -sn 192.168.11.255/24
    * /24で1分程度時間がかかった
    * 例
      ```
      # nmap -sn 192.168.11.255/24
      Starting Nmap 7.80 ( https://nmap.org ) at 2021-05-31 20:20 JST
      Nmap scan report for 192.168.11.1
      Host is up (0.012s latency).
      Nmap scan report for 192.168.11.16
      Host is up (0.15s latency).
      Nmap scan report for test (192.168.11.102)
      Host is up (0.16s latency).
      Nmap scan report for pi4 (192.168.11.103)
      Host is up (0.010s latency).
      Nmap scan report for kali (192.168.11.104)
      Host is up (0.010s latency).
      Nmap done: 256 IP addresses (5 hosts up) scanned in 68.02 seconds
      ```
      