# ufw

* raspbian OS busterで動作確認

## 導入

* apt install -y ufw

## 各種設定コマンド

* serviceの状態確認
  * systemctl status ufw
    * デフォルトでenabledになっており、基本的に有用な情報はない
* 状態確認
  * 概要
    * ufw status
      * Status: inactive の場合はufw enableで有効化可能
  * 詳細な状態確認
    * ufw status verbose
  * 番号つきでrule表示
    * ufw status numbered
* ufw有効化
  * ufw enable
* ufw無効化
  * ufw disable
* port許可
  * ufw allow 22
* port遮断
  * ufw deny 22
* portの特定プロトコル遮断
  * ufw deny 22/tcp
* port不明な特定service許容
  * ufw allow ssh
* 192.168.2.1からport30へのアクセスを拒否
  * ufw deny from 192.168.2.1 port 30
* defaultで全拒否設定
  * ufw default DENY
* localでの全リクエスト許容
  * ufw allow from 192.168.11.0/24 to 192.168.11.0/24
    * 範囲指定はnetmask使用
      * 参考) https://askubuntu.com/questions/646424/ufw-allow-range-of-ip-addresees
  * 上記コマンドで追加された設定は下記に書き込まれる
    * /etc/ufw/user.rules
      * 自動生成らしく、ファイルを直接更新してもufw reload実行時に設定ファイルは元に戻る
* 設定適用  
  * ufw enable
  * 明示的に更新時は下記実行
    * ufw reload
* ログ出力するよう設定
  * ufw logging on
  * 下記へ出力される
    * /var/log/ufw.log
* 番号指定でrule削除
  * ufw delete (番号)
  