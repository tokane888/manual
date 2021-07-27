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
    * 2021/7/28現在は下記
      * curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
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
        v16.5.0
        ```
  * version指定してインストール
    * nvm install v16.5.0
  * インストールしたバージョンを一覧表示
    * nvm list
      ```
      $ nvm list
      ->      v16.5.0
      default -> v16.5.0
      iojs -> N/A (default)
      unstable -> N/A (default)
      node -> stable (-> v16.5.0) (default)
      stable -> 16.5 (-> v16.5.0) (default)
      lts/* -> lts/fermium (-> N/A)
      lts/argon -> v4.9.1 (-> N/A)
      lts/boron -> v6.17.1 (-> N/A)
      lts/carbon -> v8.17.0 (-> N/A)
      lts/dubnium -> v10.24.1 (-> N/A)
      lts/erbium -> v12.22.3 (-> N/A)
      lts/fermium -> v14.17.3 (-> N/A)
      ```
      * 上記のようにlong term support版のnodejsへのaliasも表示される
  * 有効なバージョンを確認
    * node -v
      ```
      $ node -v
      v16.5.0
      ````