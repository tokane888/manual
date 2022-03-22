# plant uml

## インストール

* vscode拡張のPlant UMLインストール
* 下記実行でChocolateyからGraphvizインストール
  * windows(WSL2以外)の場合
    * cinst --yes Graphviz
  * WSL2の場合
    * apt install -y graphviz
* jdkインストール
  * chocolateyからのjdk11のインストールは、パッケージにエラーがあるのかfailしたためwebからインストール
  * https://java.com/ja/download/
* OS再起動
* 動作確認
  * vscodeでtest.puを作成し、下記記載
    ```
    @startuml test
    class Enemy {
        - string name
        + void Attack()
    }
    @enduml

    ```
* alt + uでプレビュー表示されること確認

## goのソースからクラス図生成

* goplantumlインストール
  * go install github.com/jfeliu007/goplantuml/cmd/goplantuml@latest
  * 参考
    * https://github.com/jfeliu007/goplantuml
* クラス図生成
  * goplantuml -recursive (解析対象のディレクトリ) > tmp.pu

## 既存DBからER図生成

* planterインストール
  * go install github.com/achiku/planter@latest
* DBへ接続してER図生成
  * planter postgres://planter@localhost/planter?sslmode=disable -o ER.uml
    * postgresのURLフォーマットは下記
      * postgresql://[user[:password]@][netloc][:port][/dbname][?param1=value1&...]