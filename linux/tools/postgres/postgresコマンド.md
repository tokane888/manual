## postgres

### ubuntuへインストール

[参考](https://symfoware.blog.fc2.com/blog-entry-2405.html)

* リポジトリ追加
  * sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
* 認証キー追加

  ```
  sudo apt-get install -y curl ca-certificates
  curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
  ```
* インストール

  ```
  sudo apt update -y
  sudo apt install -y postgresql-12
  ```
  * この時点で作成される管理ユーザー
    * User: postgres
    * Pass: (未設定) => ログイン不可

### postgresコマンド

* postgresユーザーに変更
  * su postgres
* パーミッション関連で以降のコマンドでエラーが出るのを避けるため$HOMEへ
  * cd
* service開始
  * initdの場合
    * service postgresql start
    * TODO: enable方法追記
  * systemdの場合
    * systemctl start postgresql && systemctl enable postgresql 
* testdb作成
  * createdb testdb
* DB接続
  * psql testdb
* role作成
  * create role test with login
* 外部接続許可
  * sed -i "s/^#listen_addresses = 'localhost'/listen_addresses = '*'\t/" /etc/postgresql/12/main/postgresql.conf
* 認証を受けるIPの範囲を拡大
  * echo "host    all             all             192.168.1.1/24          md5" >> /etc/postgresql/12/main/pg_hba.conf
* ユーザー作成
  * createuser --pwprompt --interactive pgadmin
* サービス名
  * postgresql
    * /lib/systemd/system/postgresql.service
* DB接続
  * psql (DB名)
  * \c (DB名)
  * 注）権限の問題でsu postgresしないと動かない
* DB切断
  * \q
* role一覧取得
  * ログイン等に使用するrole
    * \du
  * システムroleを含むrole
    * select * from pg_roles;
* DB一覧取得
  * psql -l
  * \l
* table一覧取得
  
  ```
  select schemaname, tablename, tableowner 
  from pg_tables 
  where schemaname not like 'pg_%' and schemaname != 'information_schema';
  ```