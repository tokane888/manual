# plant uml

## インストール

* vscode拡張のPlant UMLインストール
* 下記実行でChocolateyからGraphvizインストール
  * cinst --yes Graphviz
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