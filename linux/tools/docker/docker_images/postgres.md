## postgres (docker image)

https://hub.docker.com/_/postgres

### 概要

* osはDebian
  * alpine-12 等のタグが付いているのはalpineを採用している軽量版
* 各種環境変数
  * POSTGRES_DB
    * コンテナ生成時に当該の名前のDBを作成

### 環境構築

* 試験的に起動
  * docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
  