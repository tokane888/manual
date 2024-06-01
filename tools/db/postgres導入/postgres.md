# postgres

## 導入

### windows

* python実践入門本記載の手順
* 下記からone click installerをダウンロードしてデフォルト設定でインストール
  * https://www.enterprisedb.com/downloads/postgres-postgresql-downloads  
* (option)スタックビルダでソフトウェアインストール
  * スタート => PostgreSQL 11 => Application Stack Builder を管理者として実行
  * インストーラ実行時に"Launch Stack Builder at exit?"を選択していると自動できどうするとのこと
  * 特になくても動きそうなので、一旦何も選択せずにキャンセル
* PATHに下記追加
  * C:\Program Files\PostgreSQL\13\bin
    * ver番号は確認の上で記載
* データベースクラスタは、winの場合は作成済みになっているので作成手順省略
  * データベースクラスタ: データベースを格納する場所

### Ubuntu

* 参考) https://www.postgresql.org/download/linux/ubuntu/
* apt repository追加の上install
  ```
  # Create the file repository configuration:
  sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

  # Import the repository signing key:
  wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

  # Update the package lists:
  sudo apt-get update

  # Install the latest version of PostgreSQL.
  # If you want a specific version, use 'postgresql-12' or similar instead of 'postgresql':
  sudo apt-get -y install postgresql
  ```

### docker

* 下記参照
  * linux\tools\docker\docker_images\postgres.md
* テスト用postgresコンテナ作成
  * docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
  * docker exec -it some-postgres psql postgres postgres

## 動作確認

* postgresユーザーに切り替え
  * sudo -i -u postgres
    * 参考) アカウント切替なしでpostgresプロンプトを起動する場合
      * sudo -u postgres psql
        * 通常はこちらでOK
* postgresプロンプト起動確認
  * psql
* postgresプロンプト終了
  * \q
