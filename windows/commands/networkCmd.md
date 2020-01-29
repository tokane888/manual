## windowsのネットワーク関連コマンド

* wifiパスワード出力
  * git-bashで下記実行
    * netsh wlan show profile "(-_-)zzz" key=clear | grep "Key Content" | awk '{print $4}'
      * "(-_-)zzz" はSSIDに置き換え