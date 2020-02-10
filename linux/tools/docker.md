## docker

### 動確環境

ubuntu 18.04

### 最新版インストール

* [参考リンク](https://docs.docker.com/install/linux/docker-ce/ubuntu#install-docker-engine---community-1)
  * バージョン指定してインストールしたい場合は上記参照
```
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

apt-get update -y
apt-get install -y docker-ce docker-ce-cli containerd.io
```

### 各種コマンド等

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

### DockerHub

#### imageをpush

* docker login
* tagの付け替え
  * docker tag (前image名):(前tag名) (user名)/(変更後image名):(変更後tag)
* imageのpush
  * docker push (user名)/(image名):(tag)
