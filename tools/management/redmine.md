## redmine

### 導入

* 前提
  * docker-compose導入済み
* redmine用ディレクトリ作成
  * ディレクトリ配下に.env作成
    * COMPOSE_TLS_VERSION=TLSv1_2
      * docker-compose up時のエラー抑制のため
* curl -sSL https://raw.githubusercontent.com/bitnami/bitnami-docker-redmine/master/docker-compose.yml > docker-compose.yml
* データ永続化のため、volumeマウント先のディレクトリ作成
  * mkdir mariadb_data redmine_data
* docker-compose up -d
* ブラウザで下記へアクセスしてredmineのホームが表示されることを確認
  * Docker Toolbox使用時
    * Toolboxのコンソールに表示されているIP
  * Docker for windows使用時
    * localhost
* 参考) その他コンテナ使い方ガイド
  * https://github.com/bitnami/bitnami-docker-redmine#how-to-use-this-image

### 基本操作

* ブラウザでredmineへログイン
  * デフォルト
    * user: user
    * pass: bitnami1
  * パスなどのデフォルト値は下記参照
    * https://github.com/bitnami/bitnami-docker-redmine#environment-variables