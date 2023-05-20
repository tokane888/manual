# node.js

## インストール

* 参考) https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04-ja
* 検証環境: ubuntu 20.04
* 方法1: デフォルトリポジトリからインストール
  * apt install -y nodejs
* 方法2: NodeSource PPA(Personal Package Archive)を使用してAptでnodejsインストール
  * curl -sL https://deb.nodesource.com/setup_14.x -o nodesource_setup.sh | bash -
* 方法3: Node Version Managerでnodeをインストール
  * nvm公式ページでcurlで叩くurlを確認して実行
    * 公式) https://github.com/nvm-sh/nvm#installing-and-updating
    * user毎にインストールされるので、rootではなく、自分のユーザーで実行
    * 2022/3/06現在は下記
      * curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
        * nvmのインストールもupdateも同じコマンドで可能
  * 設定読み込み
    * . ~/.bashrc
  * インストール可能なnodeのversion一覧確認
    * nvm list-remote
      * 出力例
        ```
        $ nvm list-remote
        v0.1.14
        v0.1.15
        v0.1.16
        v0.1.17
        v0.1.18
        v0.1.19
        v0.1.20
        v0.1.21
        v0.1.22
        v0.1.23
        (省略)
        v17.6.0
        ```
  * version指定してインストール
    * nvm install v17.6.0
  * インストールしたバージョンを一覧表示
    * nvm list
      ```
      $ nvm list
      ->      v17.6.0
      default -> v17.6.0
      iojs -> N/A (default)
      unstable -> N/A (default)
      node -> stable (-> v17.6.0) (default)
      stable -> 17.6 (-> v17.6.0) (default)
      lts/* -> lts/gallium (-> N/A)
      lts/argon -> v4.9.1 (-> N/A)
      lts/boron -> v6.17.1 (-> N/A)
      lts/carbon -> v8.17.0 (-> N/A)
      lts/dubnium -> v10.24.1 (-> N/A)
      lts/erbium -> v12.22.10 (-> N/A)
      lts/fermium -> v14.19.0 (-> N/A)
      lts/gallium -> v16.14.0 (-> N/A)
      ```
      * 上記のようにlong term support版のnodejsへのaliasも表示される
* 有効なnodejsのバージョンを確認
  * node -v
    ```
    $ node -v
    v17.6.0
    ````
  * 他のユーザーでは有効になっていないので注意

## デバッグ環境構築

* vscodeのlaunch.jsonに下記記載
  * "outputCapture": "std"
  * debug実行時にログ出力可能にするため
  
## 静的htmlを表示

* http_serverというnpmパッケージ使うのが簡単
