## docker

### 各種コマンド等

* docker ps時のコマンド省略(...)しない方法
  * docker ps -a --no-trunc
* コンテナのログ確認
  * docker logs (コンテナID)
  * 時刻も含めて確認したい場合は下記のログ参照
    * /var/lib/docker/containers/(コンテナID)/(コンテナID)-json.log
* image検索
  * docker search (image名)
  ```
  # docker search alpine
  NAME                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
  alpine                                 A minimal Docker image based on Alpine Linux…   6037                [OK]
  mhart/alpine-node                      Minimal Node.js built on Alpine Linux           453
  anapsix/alpine-java                    Oracle Java 8 (and 7) with GLIBC 2.28 over A…   439                                     [OK]
  ```
  * tag一覧取得は標準のコマンドでは無理
    * おとなしくDocker Hub見ておく
* docker残容量確認
  * docker system df
    * 容量が足りなくなるとコンテナ上で意味不明なエラーが頻出するので注意
    ```
    $ docker images
    REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
    sample_server         0.1                 758da91348f7        2 weeks ago         367MB
    golang                1.13.6-alpine3.11   9954d1348cd8        3 weeks ago         359MB
    jrei/systemd-ubuntu   latest              5b30d83c97b7        4 months ago        158MB
    ```
* 一般userでdockerコマンド実行可能に設定
  * docker groupにubuntuユーザーを追加
    * gpasswd --add (ユーザー名) docker
      * ex) gpasswd --add ubuntu docker
