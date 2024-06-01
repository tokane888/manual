# network速度計測

## 現在の特定のnetwork interfaceの通信量を確認

* ip -s link | grep -A 5 eno1; sleep 60; ip -s link | grep -A 5 eno1
  * OS起動後の当該network interfaceの通信量が出るので、60秒後の結果と比較して計測

## speedtest-cliを使用する方法

* 検証環境
  * ラズパイ3 + buster
* インストール
  * sudo apt install -y speedtest-cli
* 実行
  * speedtest
    * 下記のエラーが出る場合がある
      ```
      root@raspberrypi:~# speedtest
      Retrieving speedtest.net configuration...
      Traceback (most recent call last):
        File "/usr/bin/speedtest", line 11, in <module>
          load_entry_point('speedtest-cli==2.0.2', 'console_scripts', 'speedtest')()
        File "/usr/lib/python3/dist-packages/speedtest.py", line 1887, in main
          shell()
        File "/usr/lib/python3/dist-packages/speedtest.py", line 1783, in shell
          secure=args.secure
        File "/usr/lib/python3/dist-packages/speedtest.py", line 1027, in __init__
          self.get_config()
        File "/usr/lib/python3/dist-packages/speedtest.py", line 1113, in get_config
          map(int, server_config['ignoreids'].split(','))
      ValueError: invalid literal for int() with base 10: ''
      ```
      * 原因
        * サーバ側更新でapiレスポンスの形式が変わったが、クライアント側が追随できていないらしい
          * 参考) https://unix.stackexchange.com/questions/644442/speedtest-cli-valueerror-invalid-literal-for-int-with-base-10
      * 設定バックアップ
        * sudo gzip -k9 /usr/lib/python3/dist-packages/speedtest.py
      * 設定編集
        * sed -i "s/^            map(int, server_config\['ignoreids'\].split(','))$/            map(int, (server_config['ignoreids'].split(',') if len(server_config['ignoreids']) else []) )/" /usr/lib/python3/dist-packages/speedtest.py
* 実行結果例
  ```
  root@raspberrypi:~# speedtest
  Retrieving speedtest.net configuration...
  Testing from NTT (153.240.159.14)...
  Retrieving speedtest.net server list...
  Selecting best server based on ping...
  Hosted by OPEN Project (via 20G SINET) (Tokyo) [4.91 km]: 46.719 ms
  Testing download speed................................................................................
  Download: 85.85 Mbit/s
  Testing upload speed......................................................................................................
  Upload: 44.85 Mbit/s
  ```
