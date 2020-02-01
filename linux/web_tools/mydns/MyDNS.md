## MyDNS

自宅の特定PCを外出先からドメイン指定でアクセス可能にする手順

### 前提

* 当該PCは自宅内LANからsshアクセス可能j

### 手順

#### ドメイン登録

* 下記へアクセス
  * https://www.mydns.jp
* 左メニューからDOMAIN INFOへ
* 下の下記項目入力
  * Domain
    * ~~.mydns.jp
  * MX
    * ~~.mydns.jp
  * Hostname
    * ~~
* Check押下
* OK押下

#### IP通知登録

* IP登録対象のPCのコンソール開く
* crontab -e
* 下記を末尾へ
  * */1  * * * * wget -q -O /dev/null http://(MyDNSのID):(MyDNSのPASS)@www.mydns.jp/login.html
    * 毎時1分にIP通知
* (確認)MyDNS左メニューから"LOG INFO"で通知ログ確認
  * https://www.mydns.jp