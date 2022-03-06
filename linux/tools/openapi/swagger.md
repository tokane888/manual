# swagger

* openapi関連ツール

## 導入

* web上で編集する場合
  * 下記から直接編集
    * https://editor.swagger.io/#
* vscode上で編集する場合
  * vscode拡張をインストール
    * 必須
      * Swagger Viewer
        * yamlを作成後、ctrl+alt+pでプレビュー表示
          * 競合する場合は変更可能
    * option
      * openapi-lint
        * 補完等
* code generator
  * WSL2(Ubuntu 20.04)上で使用する場合
    ```
    apt install -y openjdk-8-jre
    cd /usr/local/bin/
    wget https://repo1.maven.org/maven2/io/swagger/swagger-codegen-cli/2.2.0/swagger-codegen-cli-2.2.0.jar -O swagger-codegen-cli.jar
    ```
  * 動確1
    * java -jar swagger-codegen-cli.jar help
  * alias設定を.bash_aliasesに記載
    * alias swagger-codegen-cli='java -jar /usr/local/bin/swagger-codegen-cli.jar'
  * . ~/.bashrc

## stubコード生成

* server側、client側両方のソースが自動生成可能だが、あくまでもstub向け
  * document等も同時に生成される
* 適当なopenapi yamlを作成
  * ファイル名例) openapi-sensor.yaml
* 下記コマンドでgolang向けのソース生成
  * サーバ生成
    * mkdir server
    * swagger-codegen-cli generate -l go-server -i openapi-sensor.yaml -o server
    * 生成後にすぐビルド可能なわけではなく、下記が必要
      * 各.goファイルにpackage名付与
      * main.goのimport path調整
      * go mod init && go mod tidy
  * クライアント生成
    * mkdir client
    * swagger-codegen-cli generate -l go-client -i openapi-sensor.yaml -o client
    * TODO: これも恐らくそのままでは動かないので追記
