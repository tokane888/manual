# dns本

* 参考
  * DNSが良く分かる教科書

### ゾーン

* ゾーンは委任によって任された範囲
* ネームサーバで下記の情報管理
  * ドメイン名とIPの対応
  * ドメイン名と委任先のネームサーバの対応

### レジストリ

* ドメインの各階層を管理する管理者
* 以下の役割を持つ
  * レジストリデータベースの運用管理
  * 自身が登録管理するドメイン名のポリシーを定めて周知
  * ドメイン名登録申請受付
  * Whoisサービス提供
    * 自身が管理するドメイン名の情報を提供
  * ネームサーバの運用
* TLD
  * ccTLD(Community Code Top Level Domain)
    * 国や地域ごとに割り振られる
    * ex) jp, uk, cn
  * gTLD(Global Top Level Domain)
    * 国や地域に寄らない
    * ex) .com, .net, .edu
  * cbTLD(Community base Top Level Domain)
    * 特定のコミュティグループで使用するTLD
    * ex) .bank, .pharmacy
  * gTLD(Graphical Top Level Domain)
    * 各国の都市、地域名を対象としたTLD
    * ex) .tokyo, .nagoya
* jpがexample.jpの解決を求められた際にexample.jpはns1.example.jpが管理していると返している例がある
    * 返しているのはNSレコード
      * ns1.example.jpのIPは***という情報を別途Aレコードで保持している
        * 委任先のドメインが、委任先のゾーンに含まれる場合、このようなグルーレコードが必要
      * このようなNSレコードを返す際には合わせてAdditionalセクションにIPアドレスも入れて返される

### リソースレコード

* Aレコード
    * ドメインとIPv4アドレスの対応
    * 1つのドメインに対応する複数のAレコードを持ち、負荷分散を図ることも可能
* AAAAレコード
    * ドメインとIPv6アドレスの対応
    * 基本的な役割、性質等はAレコードと同様
* NSレコード
    * ドメインと委任先権威サーバのドメインとの対応
