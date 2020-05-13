## Bridging方式でのOpenVPN接続

* 動確環境
  * Ubuntu 18.04実機 + OpenVPN2.4.4
  * Windows 10(WSL Ubuntu 18.04) + OpenVPN

### 概要

* 2つの方法で認証
  * 各serverとclientが鍵ペアを持って認証
  * master認証局の証明書とkey
    * 各serverとclientの証明書で署名される
* serverとclientが双方向に認証

### master CA 証明書 & 鍵生成

* (このあとの手順で生成するものは以下)
  * master 認証局の公開鍵 & 秘密鍵
  * サーバーの公開鍵 & 秘密鍵
  * 3つのclient用の公開鍵 & 秘密鍵

#### easy-rsa導入

* server側、client側両方で実施
* 依存パッケージ導入済みであることを確認
  * apt policy openssl
* 導入
  * TODO: apt install -y easyrsa で導入可能っぽい。動かしてみる
  ```
  git clone https://github.com/OpenVPN/easy-rsa.git
  cd easy-rsa/easyrsa3/
  ```

### server側で認証局作成

* 参考にした手順
  * easy-rsa/README.quickstart.md
* ./easyrsa init-pki
  * ./pkiにPKIディレクトリが作成される
* ./easyrsa build-ca
  * パス設定
  * DN設定
    * "Common Name": Enterでデフォルト値設定

### client側でPKI構築し、keypair/request生成

* ./easyrsa init-pki
* ./easyrsa gen-req (Entity名)
  * 動確時はEntity名 = "testReq"
  * "Enter PEM pass phrase: ": パス設定
  * "Common Name": Enterでデフォルト値設定
  * keypairとrequestがpki配下に生成される
    ```
    Keypair and certificate request completed. Your files are:
    req: /home/tom/vpn/easy-rsa/easyrsa3/pki/reqs/testReq.req
    key: /home/tom/vpn/easy-rsa/easyrsa3/pki/private/testReq.key
    ```
* サーバ側に.reqファイル送信
* サーバ側で.reqファイルimport
  * pkiディレクトリがあるディレクトリで下記実行し、requestをimport
    * ./easyrsa import-req testReq.req testReq
  * requestに署名
    * ./easyrsa sign-req client testReq
* (TODO: READMEの5.Transport the newly signed certificate to the requesting entity.が良く分からん。後で追記)

### 参考) ディレクトリ構成

* easy-rsa
  * git cloneしてきたもの
* easy-rsa/easyrsa3
  * 今回使用するeasyrsa及び関連ファイルが生成される場所
* easy-rsa/easyrsa3/pki
  * 公開鍵認証基盤。init-pkiで生成される
    * init-pki時に生成されるのは下記
      * openssl-easyrsa.cnf: 証明書などのパス設定を含む
      * safessl-easyrsa.cnf: 諸々の設定+パス設定。easyrsaディレクトリなどが絶対パスで書かれており、動かすと不味そうな雰囲気
      * private(空ディレクトリ)
      * revoked(空ディレクトリ)
  * build-ca実行でファイルが追加される
    * ca.crt: CA証明書。x509.v3形式
      * 下記コマンドで証明書の内容確認可能
        * openssl x509 -in ca.crt -text
      * フォーマット詳細は下記参照
        * https://www.atmarkit.co.jp/ait/articles/0401/01/news098.html
    * certs_by_serial: 新規に作成したcertの置き場所
    * index.txt: なにかのデータベース。最初は空
    * index.txt.attr: 
    * private/ca.key: 認証局の秘密鍵
    * serial: なにかのシリアル番号。"01"とだけ書かれている
* easy-rsa/easyrsa3/x509-types/*
  * x.509 v3関連の初期設定っぽい
  * x.509 v3はPKIに関する規格
    * 証明書、証明書失効リストなどを含む