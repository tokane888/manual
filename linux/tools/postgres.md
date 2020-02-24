## postgres

### ubuntuへインストール

[参考](https://symfoware.blog.fc2.com/blog-entry-2405.html)

* リポジトリ追加
  * sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
* 認証キー追加

  ```
  sudo apt-get install curl ca-certificates
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

### postgres設定

* ユーザー作成
  * postgresユーザーに変更
    * su postgres
  * パーミッション関連で以降のコマンドでエラーが出るのを避けるため$HOMEへ
    * cd
  * testdb作成
    * createdb testdb
      * 最初からあると警告出た。その場合はそのまま進める
* DB接続
  * psql testdb
* role作成
  * create role test with login
* 外部接続許可
  * sed -i "s/^#listen_addresses = 'localhost'/listen_addresses = '*'\t/" /etc/postgresql/12/main/postgresql.Gconf
* 認証を受けるIPの範囲を拡大
  * echo "host    all             all             192.168.1.1/24          md5" >> /etc/postgresql/12/main/pg_hba.conf
* ユーザー作成
  * createuser --pwprompt --interactive pgadmin
* サービス名
  * postgresql
    * /lib/systemd/system/postgresql.service

### postgresコマンド

* 注）権限の問題でsu postgresしないと動かない
* DB接続
  * psql (DB名)
  * \c (DB名)
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