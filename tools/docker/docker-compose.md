## docker-compose

* コンテナ間共有変数
  * docker-compose.ymlの拡張フィールドを使用
    * https://docs.docker.com/compose/compose-file/#extension-fields#extension-fields
      * .ymlの頭に、"x-"で始まるフィールドを設けて定義
  * services.service.env_file で共有の環境変数ファイルを設定することも可能
