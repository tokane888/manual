## docker

### 各種コマンド等

* docker ps時のコマンド省略(...)しない方法
  * docker ps -a --no-trunc
* コンテナのログ確認
  * docker logs (コンテナID)
  * 時刻も含めて確認したい場合は下記のログ参照
    * /var/lib/docker/containers/(コンテナID)/(コンテナID)-json.log
  * ログが多い場合直近数分のログだけ出力可能
    * docker logs (コンテナID) --since 5m
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
* コンテナIP確認
  * docker container inspect (コンテナ名)
    * "Networds"配下の"IPAddress"参照
      * 注)"NetworkSettings"直下ではない
  * 下記手順でも確認可能
    * ネットワーク名確認
      * docker container inspect --format '{{.NetworkSettings.Networks}}' (コンテナ名)
    * IP確認
      * docker container inspect --format '{{.NetworkSettings.Networks.(ネットワーク名).IPAddress}}' (コンテナ名)
* :latest tagのついたdocker imageに対応するtagを探す
  * https://github.com/ryandaniels/docker-script-find-latest-image-tag

### ビルド関連コマンド

* build時引数指定
  * --build-arg (引数名1)=(value1) --build-arg (引数名2)=(value2)
  * Dockerfile内

    ```
    ARG (引数名1)
    ARG (引数名2)
    ```
* docker imageを.tar形式で出力
  * docker save (image name:tag) > ~~.tar
* .tar形式のdocker imageを読み込み
  * docker load < ~~.tar
* dockerコンテナをimage化
  * docker commit (コンテナ名) (image名):(タグ名)

### 設定

* 一般userでdockerコマンド実行可能に設定
  * docker groupにubuntuユーザーを追加
    * gpasswd --add (ユーザー名) docker
      * ex) gpasswd --add ubuntu docker
