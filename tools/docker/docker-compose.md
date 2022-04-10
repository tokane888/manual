## docker-compose

* コンテナ間共有変数
  * docker-compose.ymlの拡張フィールドを使用
    * https://docs.docker.com/compose/compose-file/#extension-fields#extension-fields
      * .ymlの頭に、"x-"で始まるフィールドを設けて定義
  * services.service.env_file で共有の環境変数ファイルを設定することも可能
* 関連コマンド
  * コンテナ等起動
    * docker-compose up -d
  * コンテナ等終了
    * docker-compose down
  * コンテナ終了時にvolumeも削除
    * docker-compose down -v