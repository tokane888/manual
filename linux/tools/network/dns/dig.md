# dig

## インストール

* ubuntu
  * apt install -y dnsutils
* cent
  * yum install -y bind-utils

## コマンド

* フルリゾルバー又は権威サーバにドメイン解決の問い合わせを行い、レスポンスのDNSメッセージを出力
* 基本形式
  * dig (@server) (domain) (type)
    * @server: DNSサーバのIP
    * domain: 問い合わせ対象のドメイン
    * type: A, AAAA等。デフォルトはA
* サンプル
  * dig @8.8.8.8 www.google.com A IN
    ```
    root@tom-PC-LZ550NSB:~# dig @8.8.8.8 www.google.com A IN

    ; <<>> DiG 9.11.3-1ubuntu1.11-Ubuntu <<>> @8.8.8.8 www.google.com A IN
    ; (1 server found)
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 58335
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 512
    ;; QUESTION SECTION:
    ;www.google.com.                        IN      A

    ;; ANSWER SECTION:
    www.google.com.         245     IN      A       172.217.25.100

    ;; Query time: 10 msec
    ;; SERVER: 8.8.8.8#53(8.8.8.8)
    ;; WHEN: Wed Apr 29 16:24:58 JST 2020
    ;; MSG SIZE  rcvd: 59
    ```
  * status: NOERRORなので正常応答
    * NXDOMAINだと、リソースレコードが当該DNS及び下の階層に存在しない
  * flags: 応答にどのフラグビットがセットされているかを示す
    * qr: 問い合わせが0。応答が1
    * rd: 名前解決要求。0は権威サーバへの問い合わせ。1はフルリゾルバーへの問い合わせ
    * ra: 1なら名前解決が可能。0なら不可
    * aa: 応答したサーバが、問い合されたドメイン名の情報に対する管理者権限を持っていれば1。今回は0
* オプション
  * +norec: 名前解決要求無効化。権威サーバへ直接問い合わせる場合に使用

