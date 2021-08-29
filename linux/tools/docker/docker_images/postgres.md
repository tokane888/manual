## postgres (docker image)

* 公式
  * https://hub.docker.com/_/postgres

### 概要

* osはDebian
  * alpine-12 等のタグが付いているのはalpineを採用している軽量版
* 各種環境変数
  * POSTGRES_DB
    * コンテナ生成時に当該の名前のDBを作成

### 環境構築

* 試験的に起動
  * docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
    * 環境変数を指定して各種設定が可能
      * 必須の環境変数はPOSTGRES_PASSWORDのみ
      * 環境変数は公式の"How to extend this image"参照
    * docker特有の環境変数はdataディレクトリが空の場合にのみ設定反映される
  * パス不要の場合の例
    * docker run --rm -d -p 11111:5432 -v postgres-data:/var/lib/postgresql/data -e POSTGRES_HOST_AUTH_METHOD=trust postgres:13.4-alpine
      * host側port 11111に設定
* コンテナ内へ
  * docker exec -it some-postgres bash
  * 
* 参考) docker-composeでの使用方法についても公式に記載あり
* 

### 環境変数一覧

* POSTGRES_PASSWORD
  * 唯一の必須の環境変数
  * POSTGRES_USERで定義された管理者、もしくはpostgresユーザーのパスワードを設定
  * postgresはtrust認証を用いているため、localhostから接続する場合にはパスワードが要求されない
  * 他のhost, containerから接続する際にはパスワードが要求される
  * コンテナ初回起動時にinitdbスクリプトによって管理者パスワードを設定
    * psqlであとからPOSTGRES_PGPASSWORDを変更しても効果はない
* POSTGRES_USER
  * POSTGRES_PASSWORDと連動し、ユーザー名とパスワードを設定
    * 管理者ユーザーと、同名のデータベースを作成
    * default: postgres
  * この変数を指定しても初期化時に下記の表示は出るが、ユーザー指定は成功している
    * The files belonging to this database system will be owned by user "postgres"
* POSTGRES_DB
  * DB名
  * default: $POSTGRES_USER(postgres)
* POSTGRES_INITDB_ARGS
  * postgres initdbに渡す引数を指定
  * 例
    * -e POSTGRES_INITDB_ARGS="--data-checksums"
* POSTGRES_INITDB_WALDIR
  * postgres transaction log配置先のパスを指定
  * default: postgres dataディレクトリ配下($PGDATAのサブディレクトリ)
* POSTGRES_HOST_AUTH_METHOD
  * 接続時の認証方法指定
  * db初期化前の場合pg_hda.confに記載追加される
    * echo "host all all all $POSTGRES_HOST_AUTH_METHOD" >> pg_hba.conf
  * trust認証はだれでもパス無しでログインできるので非推奨
    * 公式参考) https://www.postgresql.org/docs/current/auth-trust.html
  * POSTGRES_HOST_AUTH_METHOD=trust に設定すると、POSTGRES_PASSWORDは必須でなくなる
* PGDATA
  * database file配置場所
  * default: /var/lib/postgresql/data
  * volumeのマウントポイントを指定せず、そのサブディレクトリなどが推奨

### Docker secret

* 前述の環境変数の代わりに*_FILEでファイルパスを渡して読み取らせることが可能
  * 下記のDocker secretからパスワード読み取る場合が多い
    * /run/secrets/<secret_name>
  * 現状 POSTGRES_INITDB_ARGS, POSTGRES_PASSWORD, POSTGRES_USER, and POSTGRES_DB のみサポート

### 初期化スクリプト

* 追加の初期化処理を行いたい場合は下記ディレクトリ配下に*.sql, *.sql.gz, *.shを追加
  * /docker-entrypoint-initdb.d
    * ディレクトリ自体は必要に応じて作成する
    * 初期化処理はdataディレクトリが空の場合にのみ実行される
    * 初期化スクリプトは、ファイル名のアルファベット順に実行される
* initdbへpostgres userとDBを作成するように指示した後、.sqlを実行し、実行可能な.shを実行。実行不可な.shはsource

### database設定

* 詳細はpostgres公式の資料を見る
  * https://www.postgresql.org/docs/current/static/runtime-config.html
* custom config file使用
  * configファイルを作成し、コンテナの中へ
  * サンプルが必要な場合は下記参照
    * /usr/share/postgresql/postgresql.conf.sample