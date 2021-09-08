# go環境構築

## windows上での環境構築

* chocolateyで、repo上の最新golangのver確認
  * choco info golang
* chocolateyで最新版golangインストール
  * choco install golang
* 環境変数設定
  * GOPATH=

## WSL, ubuntuなどでの環境構築

* WSL上のUbuntu 20.04で検証

### gvm(go version manager)+goインストール

* gvm本体をインストール
  * bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
    * ユーザー別にインストールされるっぽい
  * セッション再起動
  * 依存パッケージも導入
    * sudo apt-get install -y curl git mercurial make binutils bison gcc build-essential
* go関連ビルド用のgo1.4をインストールし、有効化
  * gvm install go1.4 -B
    * -Bは、コンパイルを行わずバイナリを直接ダウンロードするオプション
    * go 1.5からgo自身でgoのビルドを行うようになったためまずビルド用にgo 1.4が必要
  * gvm use go1.4
* インストール可能なgoのver一覧取得
  * gvm listall
* 特定verのgoをインストールして有効化
  * gvm install go1.17
  * gvm use go1.17 --default
    * --defaultを付けておかないと、terminalから抜けた際に設定が保持されない
* ダウンロード済みのgoのver一覧取得
  * gvm list
* 現在のgoのversion確認
  * go version

## GOPATH, GOROOT関連設定

* go get
  * go 1.1からgo getは$GOROOTをパッケージダウンロード先として使用しなくなった
  * go getを使うには$GOPATHが必要
  * 下記でhelp参照可能
    * go help get
* GOPATH
  * goのライブラリなどを探す先
  * どこを指定しても良いが、一度指定したら変更しないほうが良い
  * GOPATH=~/.go にしておけば良い
* GOROOT
  * go SDK配置場所
  * 特に明示的に指定しなくても良さそう
  * gvmでgo1.17をインストールした際には下記になっていた
    * /root/.gvm/gos/go1.17

## visual studio code

* Go Team at Google製のGo拡張機能をインストール
* コマンドパレットからGo依存パッケージをすべてインストール
  * ctrl+shift+p => "Go:Install/Update Tools" と入力してenter
  * gocode, gopkgs等全項目選択して"OK"
    * ここでdelveなどのデバッグツールもインストールされる

## 新project作成時の環境構築

* 前提
  * 単純な.goファイル作成済み
  * 上記の.goはコンソールからgo runで実行すれば実行成功
  * vscodeからF5でデバッグ実行すると下記のようなエラーになる
    ```
    Build Error: go build -o /root/.go/src/learn/golang/tmp/__debug_bin -gcflags all=-N -l /root/program/golang/tmp
    go: go.mod file not found in current directory or any parent directory; see 'go help modules' (exit status 1)
    ```
* デバッグ実行環境構築
  * vscode左メニューでデバッグボタンのエイコンを押下してRUNコンソール画面へ移動
  * "create a launch.json file"選択し、launch.json作成
    * 一旦変更無しでOK
  * ソースをGOPATH配下以外に配置するとデバッグ実行時にエラーになったので当面GOPATH配下に配置
* go module対応
  * go.mod初期化
    * go mod init (remoteリポジトリ)
      * ex) go mod init github.com/tokane888/go-module-sample
  * requiredパッケージとgo.sumを追加
    * go mod tidy
      * go build時に暗黙的にgo.mod, go.sumが編集される挙動は1.16でdisableされた
    * go build, go test等の標準コマンドは、go.mod, go.sumに従って自動でmodule download

