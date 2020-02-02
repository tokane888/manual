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

* 一般userでdockerコマンド実行
  * docker groupにubuntuユーザーを追加
    * gpasswd --add (ユーザー名) docker
      * ex) gpasswd --add ubuntu docker
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

### DockerHub

#### imageをpush

* docker login
* tagの付け替え
  * docker tag (前image名):(前tag名) (user名)/(変更後image名):(変更後tag)
* imageのpush
  * docker push (user名)/(image名):(tag)
