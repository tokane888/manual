## postgres (docker image)

https://hub.docker.com/_/postgres

### 概要

* osはDebian
  * alpine-12 等のタグが付いているのはalpineを採用している軽量版
* 各種環境変数
  * POSTGRES_DB
    * コンテナ生成時に当該の名前のDBを作成