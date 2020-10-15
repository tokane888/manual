# dep

## インストール

* go get -u github.com/golang/dep/cmd/dep
  * 下記でもインストール可能だが、バージョンが古く、2020/10現在設定ファイルのフォーマットが微妙に異なるのでgo getを使用する
    * apt install -y go-dep

## プロジェクト初期化

* dep init
  * プロジェクト内に下記のファイル、ディレクトリが生成される
    * Gopkg.toml
      * 依存関係リストと取得元
    * Gopkg.lock
      * マニフェストから生成され、アプリケーションのコードを解析
    * vendor
* dep ensure
  * importしたいパッケージがvendor配下へインストールされ、Gopkg.toml, Gopkg.lockが追記される
* dep status
  * パッケージの過不足等の状態表示
