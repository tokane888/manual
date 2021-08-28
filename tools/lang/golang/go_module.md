# go module

* moduleとは
  * 単一のまとまりとして管理される、関連go packageの集合
  * 依存関係を厳密に記録し、再ビルドを可能とする
* 通常git等のrepositoryは単一のmoduleをrepository rootに持つ

## go.mod

* 参考) https://github.com/golang/go/wiki/Modules#gomod
* moduleはgo source fileのtreeとしてgo.modで定義される
* module sourceはGOPATHの外にあっても良い
* 4種類のdirectiveがある
  * module
    * 自身のmodule path
    * module内の全packageのimport pathはmodule pathを共通prefixとする
    * 下記のようにしてgithub.com/user/mymod moduleのbar packageをimport可能
      * import "github.com/user/mymod/bar"
  * require
    * 依存package
  * replace, exclude
    * main moduleにのみ影響
    * 使っているのを見たことがない。。
* importを追加し、go.modに対応するrequireが記載されていない場合
  * そのままgo build, go test等を実行するとエラー
    * 1.15までは自動で依存パッケージをdownloadしてくれたらしい
  * go mod tidy等による依存パッケージdownloadが必要
* moduleのversion
  * go.modの記載例
    * require M v1.2.3
      * M moduleの"v1.2.3" git tag以上、"v2"以下のversionに依存とされる
  * 自moduleのgo.modに下記2つのmoduleに依存する場合
    * module A: require D v1.0.0
    * module B: require D v1.1.1
    * この場合、より新しいv1.1.1をビルドに使用
* goのversionとmodule
  * go言語はver 1.14でmoduleを公式にサポート
  * go 1.16でGO111MODULE=onがデフォルトに
  * go 1.11からvgoという仕組みも標準に入っているらしい

## 関連コマンド

* go.mod初期化
  * go mod init (remoteリポジトリ)
    * ex) go mod init github.com/tokane888/go-module-sample
* requiredパッケージとgo.sumを追加
  * go mod tidy
    * go build時に暗黙的にgo.mod, go.sumが編集される挙動は1.16でdisableされた
  * go build, go test等の標準コマンドは、go.mod, go.sumに従って自動でmodule download
* 直接及び関節依存moduleの最新ver一覧表示
  * go list -u -m all
* 使用module一覧表示
  * go list -m all
* module update
  * go get -u (repo url)
* 既存のprojectにgo.modを追加する場合
  * 下記のどちらかの方法でmodule modeを有効化
    * $GOPATH/srcの外のmodule source treeのrootへ移動
      * $GOPATHの外ではGO111MODULEを設定してmodule modeを有効化する必要がないため
    * export GO111MODULE=on
      * 上記設定後下記で移動
        * cd $GOPATH/src/...(当該reposiroty)
  * moduleの定義をgo.modに記載    
    * go mod init
  * 
