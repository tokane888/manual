## go get

* go関連ライブラリインストール
  * go get -u (url)
    * 例) go get -u github.com/golang/dep/cmd/dep
    * -u: 新しいマイナーリリース又はパッチリリースが利用可能な場合にパッケージと依存パッケージをネットワークから更新

## go clean

* go関連ライブラリアンインストール
  * go clean -i (url)
    * -i: インストールされたarchive, binaryを削除(go getの反対)
    * -n: アンインストール時に実行するコマンドを表示。実際には実行しない
    * $GOPATH/src配下は削除されないため、手動での削除が必要